METHOD precheck_anlageaendern.

    LOOP AT keys INTO DATA(ls_key).

      " 1. Read THIS specific service (not all services at once)
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
        FIELDS ( referenceinternalid referenceid serviceuuid )
        WITH VALUE #( ( serviceuuid = ls_key-serviceuuid ) )
        RESULT DATA(lt_services).

      CHECK lt_services IS NOT INITIAL.
      
      DATA(ls_service) = lt_services[ 1 ].  " Now this is correct - we read only ONE service

      " Get order details from backend view
      SELECT SINGLE *
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @ls_service-referenceid
        INTO @DATA(ls_order).

      IF sy-subrc <> 0.
        CONTINUE.
      ENDIF.

      " 2. Check for Fixed Disposal Contract
      IF ls_order-entsorgunganlage IS NOT INITIAL.
        SELECT SINGLE zz_framework_agreem
          FROM ewa_el_wdplant
          WHERE wdplantnr = @ls_order-entsorgunganlage
          INTO @DATA(lv_existing_contract).

        IF sy-subrc = 0 AND lv_existing_contract IS NOT INITIAL.
          APPEND VALUE #( %tky = ls_key-%tky
                          %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
          APPEND VALUE #( %tky = ls_key-%tky
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

      " 3. Validate Selected Facility
      IF ls_key-%param-anlage IS INITIAL.
        APPEND VALUE #( %tky = ls_key-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = ls_key-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '011'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = 'Keine Entsorgungsanlage ausgewählt' )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      SELECT SINGLE wdplantnr, zz_available_dispo, zz_framework_agreem
        FROM ewa_el_wdplant
        WHERE wdplantnr = @ls_key-%param-anlage
        INTO @DATA(ls_plant).

      IF sy-subrc <> 0.
        APPEND VALUE #( %tky = ls_key-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = ls_key-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '012'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = 'Entsorgungsanlage nicht gefunden'
                                 v2       = CONV #( ls_key-%param-anlage ) )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      IF ls_plant-zz_available_dispo IS INITIAL OR ls_plant-zz_available_dispo <> 'X'.
        APPEND VALUE #( %tky = ls_key-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = ls_key-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '013'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = 'Anlage nicht verfügbar für Dispo'
                                 v2       = CONV #( ls_key-%param-anlage ) )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " 4. AVV-Code Validation
      SELECT COUNT(*)
        FROM /watp/twdplanavv
        WHERE wdplantnr = @ls_key-%param-anlage
        INTO @DATA(lv_avv_count).

      IF lv_avv_count > 0.
        SELECT SINGLE avvcode
          FROM ewa_order_object
          WHERE pobjnr = @ls_service-referenceinternalid
          INTO @DATA(lv_order_avvcode).

        IF sy-subrc = 0 AND lv_order_avvcode IS NOT INITIAL.
          SELECT SINGLE avvcode
            FROM /watp/twdplanavv
            WHERE wdplantnr = @ls_key-%param-anlage
              AND avvcode = @lv_order_avvcode
            INTO @DATA(lv_found_avv).

          IF sy-subrc <> 0.
            APPEND VALUE #( %tky = ls_key-%tky
                            %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
            APPEND VALUE #( %tky = ls_key-%tky
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
      
      " Clear variables for next iteration
      CLEAR: ls_service, ls_order, lv_existing_contract, ls_plant, 
             lv_avv_count, lv_order_avvcode, lv_found_avv, lt_services.

    ENDLOOP.

ENDMETHOD.


METHOD anlageaendern.

    LOOP AT keys INTO DATA(ls_key).

      " Read THIS specific service (not all at once)
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
        FIELDS ( serviceuuid referenceinternalid referenceid )
        WITH VALUE #( ( serviceuuid = ls_key-serviceuuid ) )
        RESULT DATA(lt_services).

      CHECK lt_services IS NOT INITIAL.
      
      DATA(ls_service) = lt_services[ 1 ].  " Now this is correct

      " Check if ExtCustom exists for THIS service
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        FIELDS ( serviceuuid )
        WITH VALUE #( ( serviceuuid = ls_key-serviceuuid ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
        DATA: lv_cid        TYPE string VALUE '$abap_cid_anlage_',
              lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.

        APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
        <ls_ext_create>-serviceuuid = ls_key-serviceuuid.
        <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
          CREATE BY \_extcustom
          AUTO FILL CID SET FIELDS WITH lt_ext_create
          MAPPED DATA(lmapped_create)
          FAILED DATA(lfailed_create)
          REPORTED DATA(lreported_create).

        IF lfailed_create IS NOT INITIAL.
          APPEND LINES OF lfailed_create-extcustom TO failed-extcustom.
          APPEND LINES OF lreported_create-extcustom TO reported-extcustom.
        ENDIF.

        CLEAR: lt_ext_create, lmapped_create, lfailed_create, lreported_create.
      ENDIF.

      " Update THIS service's disposal facility
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        UPDATE FIELDS ( zz_ent_anlage )
        WITH VALUE #( ( %key-serviceuuid = ls_key-serviceuuid
                        zz_ent_anlage = ls_key-%param-anlage ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      IF lt_failed_update IS NOT INITIAL.
        APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
        APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.
      ELSE.
        " Success message for THIS service
        APPEND VALUE #( %tky = ls_key-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '015'
                                 severity = if_abap_behv_message=>severity-success
                                 v1       = 'Entsorgungsanlage erfolgreich geändert'
                                 v2       = CONV #( ls_key-%param-anlage ) )
                      ) TO reported-service.
      ENDIF.

      " Clear variables for next iteration
      CLEAR: lt_failed_update, lt_reported_update, ls_service, 
             lt_services, lt_extcustom.

    ENDLOOP.

    " Now read ALL updated services at once for the result
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky   = srv-%tky
                                                      %param = srv ) ).

ENDMETHOD.
