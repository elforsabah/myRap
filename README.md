METHOD anlageaendern.

  DATA: lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
        lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom,
        lt_srv_update TYPE TABLE FOR UPDATE /plce/r_pdservice.  " ✅ NEW

  " Read ALL services at once
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service
    FIELDS ( serviceuuid referenceinternalid referenceid functionallocation )  " ✅ Added functionallocation
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  " Loop through SERVICES
  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

    DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

    " ✅ NEW: Get disposal facility details including TPLNR
    IF ls_param-anlage IS NOT INITIAL.
      SELECT SINGLE wdplantnr, tplnr
        FROM ewa_el_wdplant
        WHERE wdplantnr = @ls_param-anlage
        INTO @DATA(ls_facility).

      IF sy-subrc = 0.
        " ✅ NEW: Prepare Service update with FunctionalLocation
        APPEND VALUE #(
          %key-serviceuuid = <ls_service>-serviceuuid
          functionallocation = ls_facility-tplnr
        ) TO lt_srv_update.
      ENDIF.
    ENDIF.

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

    " Prepare ExtCustom update
    APPEND VALUE #( %key-serviceuuid = <ls_service>-serviceuuid
                    wdplantnr = ls_param-anlage )
      TO lt_ext_update.

    CLEAR: lt_extcustom, ls_facility.

  ENDLOOP.

  " ✅ NEW: Bulk update Service entities with FunctionalLocation
  " This triggers the determination!
  IF lt_srv_update IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      UPDATE FIELDS ( functionallocation )
      WITH lt_srv_update
      FAILED DATA(lt_srv_failed)
      REPORTED DATA(lt_srv_reported).

    IF lt_srv_failed IS NOT INITIAL.
      APPEND LINES OF lt_srv_failed-service TO failed-service.
      APPEND LINES OF lt_srv_reported-service TO reported-service.
    ENDIF.
  ENDIF.

  " Bulk create missing ExtCustom records
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

  " Bulk update disposal facility in ExtCustom
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
      " Success messages
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
    ENDIF.
  ENDIF.

  " Return updated data to UI
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

  result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

ENDMETHOD.
