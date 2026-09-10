  METHOD tourgeneratedocument.

    DATA:
      lt_srv_all       TYPE zpd_service_tt,
      lt_services_tour TYPE zpd_service_tt,
      lt_tour_create   TYPE TABLE FOR CREATE /plce/r_pdtour\_Attachments,
      lt_srv_create    TYPE TABLE FOR CREATE /plce/r_pdservice\_Attachments,
      lv_ok            TYPE i,
      lv_err           TYPE i,
      lv_removed       TYPE i,
      lv_cid           TYPE i.

    DATA(lo_misc) = zcl_wr_misc_pd=>get_instance( ).

*----------------------------------------------------------------------*
* The complete print configuration goes in. build_for_tour() reduces it
* per tour template via ZWR_PDFORMS; create_manual_service_attm()
* resolves the service forms itself via ZWR_PDFORMS_SRV.
*----------------------------------------------------------------------*
    SELECT * FROM zc_printconfig INTO TABLE @DATA(lt_all_config).

    IF lt_all_config IS INITIAL.
      LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_k>).
        APPEND VALUE #( %tky = <ls_k>-%tky ) TO failed-tour.
        APPEND VALUE #( %tky = <ls_k>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                            number   = '041'
                                            severity = if_abap_behv_message=>severity-error )
                      ) TO reported-tour.
      ENDLOOP.
      RETURN.
    ENDIF.

*--- 1  the selected tours ---------------------------------------------
    READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY tour
        FIELDS ( touruuid tourid tourtemplate )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_tours)
      FAILED DATA(lt_read_failed).

    " an error in REPORTED is downgraded to a warning unless the key is
    " also in FAILED — same trap as in touranBMSfreigeben
    LOOP AT lt_read_failed-tour ASSIGNING FIELD-SYMBOL(<ls_rf>).
      APPEND VALUE #( %tky = <ls_rf>-%tky ) TO failed-tour.
      APPEND VALUE #( %tky = <ls_rf>-%tky
                      %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                          number   = '024'
                                          severity = if_abap_behv_message=>severity-error )
                    ) TO reported-tour.
    ENDLOOP.

    CHECK lt_tours IS NOT INITIAL.

*--- 2  the services of all selected tours, read once -------------------
    READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY tour BY \_serviceassignments
        FIELDS ( touruuid serviceuuid removed )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_asgmts).

    DELETE lt_asgmts WHERE removed IS NOT INITIAL.

    IF lt_asgmts IS NOT INITIAL.

      READ ENTITIES OF /plce/r_pdservice
        ENTITY service
          FIELDS ( serviceuuid servicetype )
          WITH VALUE #( FOR ls_asg IN lt_asgmts ( serviceuuid = ls_asg-serviceuuid ) )
        RESULT DATA(lt_srv).

      READ ENTITIES OF /plce/r_pdservice
        ENTITY service BY \_extwaste
          FIELDS ( serviceuuid material )
          WITH CORRESPONDING #( lt_srv )
        RESULT DATA(lt_mat).

      lt_srv_all = VALUE #(
        FOR ls_s IN lt_srv
        ( service_uuid = ls_s-serviceuuid
          service_type = ls_s-servicetype
          material     = VALUE #( lt_mat[ serviceuuid = ls_s-serviceuuid ]-material OPTIONAL ) ) ).

    ENDIF.

*--- 3  per tour --------------------------------------------------------
    LOOP AT lt_tours ASSIGNING FIELD-SYMBOL(<ls_tour>).

      CLEAR: lv_ok, lv_err, lv_removed,
             lt_services_tour, lt_tour_create, lt_srv_create.

      DATA(lv_tour_out) = condense( |{ <ls_tour>-tourid ALPHA = OUT }| ).

*     3a  tour documents
      DATA(ls_conf) = lo_misc->build_for_tour(
                        is_tour       = CORRESPONDING #( <ls_tour> )
                        it_sel_config = lt_all_config ).

      IF ls_conf IS NOT INITIAL.

        " render FIRST — nothing is deleted until there is a replacement
        DATA(lt_tour_attm) = lo_misc->create_manual_tour_attm(
                               is_tour_with_conf = ls_conf ).

        IF lt_tour_attm IS NOT INITIAL.

*         lv_removed = remove_obsolete_tour( iv_touruuid = <ls_tour>-touruuid
*                                            it_new      = lt_tour_attm ).

          " build one CREATE-BY row for this tour, with all its documents
          DATA ls_tour_create LIKE LINE OF lt_tour_create.
          CLEAR ls_tour_create.
          ls_tour_create-%tky = <ls_tour>-%tky.

          LOOP AT lt_tour_attm ASSIGNING FIELD-SYMBOL(<ls_ta>).
            lv_cid = lv_cid + 1.
            APPEND VALUE #( %cid       = |T{ lv_cid }|
                            filename   = <ls_ta>-filename
                            mimetype   = <ls_ta>-mimetype
                            attachment = <ls_ta>-attachment
                            comments   = <ls_ta>-comments ) TO ls_tour_create-%target.
          ENDLOOP.

          APPEND ls_tour_create TO lt_tour_create.

          MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
            ENTITY tour
              CREATE BY \_Attachments
                FIELDS ( filename mimetype attachment comments )
                WITH lt_tour_create
            FAILED DATA(lt_fail_t).

          " test the whole structure — the child alias cannot be named safely
          IF lt_fail_t IS NOT INITIAL.
            lv_err = lv_err + 1.
            APPEND VALUE #( %tky = <ls_tour>-%tky
                            %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                                number   = '038'
                                                severity = if_abap_behv_message=>severity-error
                                                v1       = lv_tour_out )
                          ) TO reported-tour.
          ELSE.
            lv_ok = lv_ok + lines( lt_tour_attm ).
            APPEND VALUE #( %tky = <ls_tour>-%tky
                            %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                                number   = '036'
                                                severity = if_abap_behv_message=>severity-success
                                                v1       = |{ lines( lt_tour_attm ) }|
                                                v2       = lv_tour_out )
                          ) TO reported-tour.
          ENDIF.

          IF lv_removed > 0.
            APPEND VALUE #( %tky = <ls_tour>-%tky
                            %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                                number   = '039'
                                                severity = if_abap_behv_message=>severity-information
                                                v1       = |{ lv_removed }|
                                                v2       = lv_tour_out )
                          ) TO reported-tour.
          ENDIF.

        ENDIF.
      ENDIF.

*     3b  service documents
      lt_services_tour = VALUE #(
        FOR ls_asg2 IN lt_asgmts WHERE ( touruuid = <ls_tour>-touruuid )
        ( VALUE #( lt_srv_all[ service_uuid = ls_asg2-serviceuuid ] OPTIONAL ) ) ).

      DELETE lt_services_tour WHERE service_uuid IS INITIAL.

      IF lt_services_tour IS NOT INITIAL.

        DATA(lt_srv_attm) = lo_misc->create_manual_service_attm(
                              it_selected_services = lt_services_tour ).

        IF lt_srv_attm IS NOT INITIAL.

          DATA ls_srv_create LIKE LINE OF lt_srv_create.

          LOOP AT lt_srv_attm ASSIGNING FIELD-SYMBOL(<ls_sa>)
               GROUP BY ( uuid = <ls_sa>-serviceuuid )
               ASSIGNING FIELD-SYMBOL(<grp_srv>).

*           lv_removed = lv_removed + remove_obsolete_service(
*                                       iv_serviceuuid = <grp_srv>-uuid
*                                       it_new         = lt_srv_attm ).

            CLEAR ls_srv_create.
            ls_srv_create-serviceuuid = <grp_srv>-uuid.

            LOOP AT GROUP <grp_srv> ASSIGNING FIELD-SYMBOL(<ls_member>).
              lv_cid = lv_cid + 1.
              APPEND VALUE #( %cid       = |S{ lv_cid }|
                              filename   = <ls_member>-filename
                              mimetype   = <ls_member>-mimetype
                              attachment = <ls_member>-attachment
                              comments   = <ls_member>-comments ) TO ls_srv_create-%target.
            ENDLOOP.

            APPEND ls_srv_create TO lt_srv_create.

          ENDLOOP.

          MODIFY ENTITIES OF /plce/r_pdservice
            ENTITY service
              CREATE BY \_Attachments
                FIELDS ( filename mimetype attachment comments )
                WITH lt_srv_create
            FAILED DATA(lt_fail_s).

          IF lt_fail_s IS NOT INITIAL.
            lv_err = lv_err + 1.
            APPEND VALUE #( %tky = <ls_tour>-%tky
                            %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                                number   = '040'
                                                severity = if_abap_behv_message=>severity-error
                                                v1       = lv_tour_out )
                          ) TO reported-tour.
          ELSE.
            lv_ok = lv_ok + lines( lt_srv_attm ).
            APPEND VALUE #( %tky = <ls_tour>-%tky
                            %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                                number   = '037'
                                                severity = if_abap_behv_message=>severity-success
                                                v1       = |{ lines( lt_srv_attm ) }|
                                                v2       = lv_tour_out )
                          ) TO reported-tour.
          ENDIF.

        ENDIF.
      ENDIF.

*     3c  nothing configured — information, not an error
      IF lv_ok = 0 AND lv_err = 0.
        APPEND VALUE #( %tky = <ls_tour>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                            number   = '035'
                                            severity = if_abap_behv_message=>severity-information
                                            v1       = <ls_tour>-tourtemplate
                                            v2       = lv_tour_out )
                      ) TO reported-tour.
        CONTINUE.
      ENDIF.

*     3d  outcome — a partial result deliberately does NOT fail
      IF lv_ok = 0 AND lv_err > 0.
        APPEND VALUE #( %tky = <ls_tour>-%tky ) TO failed-tour.
      ELSE.
        APPEND VALUE #( %tky   = <ls_tour>-%tky
                        %param = CORRESPONDING #( <ls_tour> ) ) TO result.
      ENDIF.

    ENDLOOP.

  ENDMETHOD.
