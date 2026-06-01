METHOD anlageaendern.

  " =========================================================================
  " Declare bulk operation tables
  " =========================================================================
  DATA: lt_ext_create    TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
        lt_ext_update    TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom,
        lt_service_uuids TYPE TABLE OF /plce/de_service_uuid.

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

    " Get the parameter for THIS service
    DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

    " Collect service UUIDs for later tour processing
    APPEND <ls_service>-serviceuuid TO lt_service_uuids.

    " Check if ExtCustom exists for this service
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      FIELDS ( serviceuuid )
      WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
      RESULT DATA(lt_extcustom).

    IF lt_extcustom IS INITIAL.
      " ExtCustom doesn't exist - prepare for creation
      DATA(lv_cid) = |$cid_{ <ls_service>-serviceuuid }$|.

      APPEND VALUE #( serviceuuid = <ls_service>-serviceuuid
                      %target = VALUE #( ( %cid = lv_cid ) ) )
        TO lt_ext_create.
    ENDIF.

    " Prepare the update (will be applied after creation if needed)
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
      " Success - add messages for each service
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
          " Facility was cleared
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
      " ✅ NEW: Recalculate Tour Route (Spur ermitteln) for assigned tours
      " ---------------------------------------------------------------------
      IF lt_service_uuids IS NOT INITIAL.

        " Find all tours assigned to these services
        SELECT DISTINCT tour_uuid
          FROM /plce/tpdsrvtsk
          WHERE service_uuid IN @lt_service_uuids
            AND tour_uuid IS NOT INITIAL
          INTO TABLE @DATA(lt_tour_uuids).

        IF lt_tour_uuids IS NOT INITIAL.

          " Prepare keys for GenerateGeoRoute action
          DATA: lt_tour_keys TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour~generategeoroute.

          LOOP AT lt_tour_uuids INTO DATA(lv_tour_uuid).
            APPEND VALUE #( %tky-touruuid = lv_tour_uuid ) TO lt_tour_keys.
          ENDLOOP.

          " Call GenerateGeoRoute action on all affected tours
          MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
            ENTITY tour
              EXECUTE generategeoroute FROM lt_tour_keys
            FAILED DATA(lt_tour_failed)
            REPORTED DATA(lt_tour_reported).

          IF lt_tour_failed IS INITIAL.
            " Success - add message about tour recalculation
            LOOP AT lt_services ASSIGNING <ls_service>.

              " Only add message if service has a tour
              SELECT SINGLE tour_uuid
                FROM /plce/tpdsrvtsk
                WHERE service_uuid = @<ls_service>-serviceuuid
                  AND tour_uuid IS NOT INITIAL
                INTO @DATA(lv_check_tour).

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

          ELSE.
            " Tour recalculation had issues - log but don't fail the action
            LOOP AT lt_tour_reported-tour ASSIGNING FIELD-SYMBOL(<ls_tour_msg>).
              " Add tour error messages to first service
              IF lt_services IS NOT INITIAL.
                APPEND VALUE #( %tky = lt_services[ 1 ]-%tky
                                %msg = <ls_tour_msg>-%msg
                              ) TO reported-service.
              ENDIF.
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
