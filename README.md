METHOD anlageaendern.

  " =========================================================================
  " Declare ALL variables at the top
  " =========================================================================
  DATA: lt_ext_create  TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
        lt_ext_update  TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom,
        lt_tour_uuids  TYPE TABLE OF /plce/r_pdtour-TourUUID,
        lt_tour_keys   TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour~generategeoroute,
        lv_tour_uuid   TYPE /plce/r_pdtour-TourUUID,
        lv_check_tour  TYPE /plce/r_pdtour-TourUUID.

  " =========================================================================
  " Read ALL services at once
  " =========================================================================
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service
    FIELDS ( serviceuuid referenceinternalid referenceid )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  " =========================================================================
  " Loop through SERVICES - prepare ExtCustom create/update
  " =========================================================================
  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

    DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

    " Check if ExtCustom exists
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      FIELDS ( serviceuuid )
      WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
      RESULT DATA(lt_extcustom).

    IF lt_extcustom IS INITIAL.
      DATA(lv_cid) = |$cid_{ <ls_service>-serviceuuid }$|.

      APPEND VALUE #( serviceuuid = <ls_service>-serviceuuid
                      %target = VALUE #( ( %cid = lv_cid ) ) )
        TO lt_ext_create.
    ENDIF.

    APPEND VALUE #( %key-serviceuuid = <ls_service>-serviceuuid
                    wdplantnr = ls_param-anlage )
      TO lt_ext_update.

    CLEAR lt_extcustom.

  ENDLOOP.

  " =========================================================================
  " Bulk create missing ExtCustom records
  " =========================================================================
  IF lt_ext_create IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      CREATE BY \_extcustom
      AUTO FILL CID SET FIELDS WITH lt_ext_create
      MAPPED DATA(lmapped)
      FAILED DATA(lfailed)
      REPORTED DATA(lreported).

    IF lfailed IS NOT INITIAL.
      APPEND LINES OF lfailed-extcustom TO failed-extcustom.
      APPEND LINES OF lreported-extcustom TO reported-extcustom.
    ENDIF.
  ENDIF.

  " =========================================================================
  " Bulk update disposal facility
  " =========================================================================
  IF lt_ext_update IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      UPDATE FIELDS ( wdplantnr )
      WITH lt_ext_update
      FAILED DATA(lt_failed_update)
      REPORTED DATA(lt_reported_update).

    IF lt_failed_update IS NOT INITIAL.
      APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
      APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.
    ELSE.
      " ---------------------------------------------------------------------
      " Success messages
      " ---------------------------------------------------------------------
      LOOP AT lt_services ASSIGNING <ls_service>.
        DATA(ls_param_msg) = keys[ %tky = <ls_service>-%tky ]-%param.

        IF ls_param_msg-anlage IS NOT INITIAL.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '015'
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = <ls_service>-referenceid )
                        ) TO reported-service.
        ELSE.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '016'
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = <ls_service>-referenceid )
                        ) TO reported-service.
        ENDIF.
      ENDLOOP.

      " ---------------------------------------------------------------------
      "Recalculate Tour Route (Spur ermitteln) for assigned tours
      "Using FOR ALL ENTRIES with lt_services directly
      " ---------------------------------------------------------------------
      IF lt_services IS NOT INITIAL.

        SELECT DISTINCT tour_uuid
          FROM /plce/tpdsrvtsk
          FOR ALL ENTRIES IN @lt_services
          WHERE service_uuid = @lt_services-serviceuuid
            AND tour_uuid IS NOT INITIAL
          INTO TABLE @lt_tour_uuids.

        IF lt_tour_uuids IS NOT INITIAL.

          " Prepare keys for GenerateGeoRoute action
          LOOP AT lt_tour_uuids INTO lv_tour_uuid.
            APPEND VALUE #( %tky-touruuid = lv_tour_uuid ) TO lt_tour_keys.
          ENDLOOP.

          " Call GenerateGeoRoute action
          MODIFY ENTITIES OF /plce/r_pdtour
            ENTITY tour
              EXECUTE generategeoroute FROM lt_tour_keys
            FAILED DATA(lt_tour_failed)
            REPORTED DATA(lt_tour_reported).

          IF lt_tour_failed-tour IS INITIAL.
            " Add message for each service that was on a tour
            LOOP AT lt_services ASSIGNING <ls_service>.

              SELECT SINGLE tour_uuid
                FROM /plce/tpdsrvtsk
                WHERE service_uuid = @<ls_service>-serviceuuid
                  AND tour_uuid IS NOT INITIAL
                INTO @lv_check_tour.

              IF sy-subrc = 0.
                APPEND VALUE #( %tky = <ls_service>-%tky
                                %msg = new_message(
                                         id       = 'Z_MSG_SVR_TOUR_EXT'
                                         number   = '017'
                                         severity = if_abap_behv_message=>severity-information
                                         v1       = <ls_service>-referenceid )
                              ) TO reported-service.
              ENDIF.

              CLEAR lv_check_tour.
            ENDLOOP.
          ENDIF.

        ENDIF.

      ENDIF.

    ENDIF.
  ENDIF.

  " =========================================================================
  " Return updated data to UI
  " =========================================================================
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

  result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

ENDMETHOD.
