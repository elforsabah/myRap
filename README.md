METHOD createtour.

  DATA lv_cid TYPE abp_behv_cid.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
       GROUP BY ( template = <key>-%param-tour_template
                  date     = <key>-%param-start_date )
       ASSIGNING FIELD-SYMBOL(<group>).

    " Generate a unique CID for this inner instance-creating operation
    lv_cid = cl_system_uuid=>create_uuid_x16_static( ).

    " Create new tour from template (child action)
    MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour
        EXECUTE createTourWithTemplate
        FROM VALUE #(
          ( %cid                 = lv_cid
            %param-without_draft = 'X'
            %param-tour_template = <group>-template
            %param-start_date    = <group>-date ) )
      MAPPED   DATA(mapped_tour)
      FAILED   DATA(failed_tour)
      REPORTED DATA(reported_tour).

    IF failed_tour IS INITIAL.

      " Read created tours to build result and message
      READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          ALL FIELDS
          WITH CORRESPONDING #( mapped_tour-tour )
        RESULT DATA(tours).

      IF lines( tours ) > 0.
        " Success message for first created tour
        INSERT NEW /plce/cx_pd_exception(
                 textid   = /plce/cx_pd_exception=>tour_confirmed
                 severity = if_abap_behv_message=>severity-success
                 tour     = tours[ 1 ]-TourId )
          INTO TABLE reported-%other.
      ENDIF.

      " Static action result: only %param
      result = VALUE #( BASE result
                        FOR tour IN tours
                        ( %param = tour ) ).

    ELSE.
      " Optional: mark failure on parent action
      " failed-createtour = VALUE #(
      "   FOR k IN keys ( %cid = k-%cid ) ).
    ENDIF.

  ENDLOOP.

ENDMETHOD.
