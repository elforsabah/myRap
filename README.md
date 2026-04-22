METHOD precheck_anlageaendern.

    " Read ALL services at once (same pattern as terminateservice)
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid referenceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " Loop through SERVICES (not keys!)
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      
      " Get the parameter for THIS service
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

      " Get order details from backend view
      SELECT SINGLE *
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @<ls_service>-referenceid
        INTO @DATA(ls_order).

      IF sy-subrc <> 0.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 1: Check for Fixed Disposal Contract
      " ---------------------------------------------------------------------
      IF ls_order-entsorgunganlage IS NOT INITIAL.
        SELECT SINGLE zz_framework_agreem
          FROM ewa_el_wdplant
          WHERE wdplantnr = @ls_order-entsorgunganlage
          INTO @DATA(lv_existing_contract).

        IF sy-subrc = 0 AND lv_existing_contract IS NOT INITIAL.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '010'
                                   severity = if_abap_behv_message=>severity-error
                                   v1       = 'Änderung nicht erlaubt'
                                   v2       = 'Fester Entsorgungsvertrag vorhanden' )
                        ) TO reported-service.
          CONTINUE.
        ENDIF.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 2: Check if facility is selected
      " ---------------------------------------------------------------------
      IF ls_param-anlage IS INITIAL.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '011'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = 'Keine Entsorgungsanlage ausgewählt' )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 3: Check facility exists and is available
      " ---------------------------------------------------------------------
      SELECT SINGLE wdplantnr, zz_available_dispo, zz_framework_agreem
        FROM ewa_el_wdplant
        WHERE wdplantnr = @ls_param-anlage
        INTO @DATA(ls_plant).

      IF sy-subrc <> 0.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '012'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = 'Entsorgungsanlage nicht gefunden'
                                 v2       = CONV #( ls_param-anlage ) )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      IF ls_plant-zz_available_dispo IS INITIAL OR ls_plant-zz_available_dispo <> 'X'.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '013'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = 'Anlage nicht verfügbar für Dispo'
                                 v2       = CONV #( ls_param-anlage ) )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 4: AVV-Code matching
      " ---------------------------------------------------------------------
      SELECT COUNT(*)
        FROM /watp/twdplanavv
        WHERE wdplantnr = @ls_param-anlage
        INTO @DATA(lv_avv_count).

      IF lv_avv_count > 0.
        " AVV assignments exist - must validate
        SELECT SINGLE avvcode
          FROM ewa_order_object
          WHERE pobjnr = @<ls_service>-referenceinternalid
          INTO @DATA(lv_order_avvcode).

        IF sy-subrc = 0 AND lv_order_avvcode IS NOT INITIAL.
          SELECT SINGLE avvcode
            FROM /watp/twdplanavv
            WHERE wdplantnr = @ls_param-anlage
              AND avvcode = @lv_order_avvcode
            INTO @DATA(lv_found_avv).

          IF sy-subrc <> 0.
            APPEND VALUE #( %tky = <ls_service>-%tky
                            %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
            APPEND VALUE #( %tky = <ls_service>-%tky
                            %msg = new_message(
                                     id       = 'Z_MSG_SVR_TOUR_EXT'
                                     number   = '014'
                                     severity = if_abap_behv_message=>severity-error
                                     v1       = 'AVV-Code nicht erlaubt für diese Anlage'
                                     v2       = CONV #( lv_order_avvcode ) )
                          ) TO reported-service.
            CONTINUE.
          ENDIF.
        ENDIF.
      ENDIF.
      " If no AVV assignments (lv_avv_count = 0), all waste types allowed

    ENDLOOP.

ENDMETHOD.


METHOD anlageaendern.

    " Declare bulk operation tables (same pattern as terminateservice)
    DATA: lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
          lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom.

    " Read ALL services at once (same pattern as terminateservice)
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid referenceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " Loop through SERVICES (not keys!) - same pattern as terminateservice
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      
      " Get the parameter for THIS service
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

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
                      zz_ent_anlage = ls_param-anlage ) 
        TO lt_ext_update.

      CLEAR lt_extcustom.

    ENDLOOP.

    " Bulk create missing ExtCustom records (same pattern as terminateservice)
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

    " Bulk update disposal facility (same pattern as terminateservice)
    IF lt_ext_update IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        UPDATE FIELDS ( zz_ent_anlage )
        WITH lt_ext_update
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      IF lt_failed_update IS NOT INITIAL.
        APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
        APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.
      ELSE.
        " Success - add messages for each service
        LOOP AT lt_services ASSIGNING <ls_service>.
          DATA(ls_param_msg) = keys[ %tky = <ls_service>-%tky ]-%param.
          
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '015'
                                   severity = if_abap_behv_message=>severity-success
                                   v1       = 'Entsorgungsanlage erfolgreich geändert'
                                   v2       = CONV #( ls_param_msg-anlage ) )
                        ) TO reported-service.
        ENDLOOP.
      ENDIF.
    ENDIF.

    " Return updated data to UI (same pattern as terminateservice)
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

ENDMETHOD.
