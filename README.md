METHOD terminateService.
    DATA: assigned_services TYPE TABLE FOR ACTION IMPORT /PLCE/R_PDTour\\ServiceAssignment~UnAssign,
          lt_service_update TYPE TABLE FOR UPDATE /PLCE/R_PDService,
          lt_history_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_StatusHistory.

    " 1. Read the service to get POBJNR (ReferenceInternalId)
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      FIELDS ( ServiceUUID ReferenceInternalId ServiceStatus ServiceId )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " 2. Unassign from Tour (if it is currently planned)
    SELECT asgmt~ServiceUUID, asgmt~TourUUID 
      FROM /PLCE/R_PDTourServiceAsgmt AS asgmt
      INNER JOIN @keys AS keys ON asgmt~ServiceUUID = keys~ServiceUUID 
      INTO CORRESPONDING FIELDS OF TABLE @assigned_services.

    IF assigned_services IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDTour
        ENTITY ServiceAssignment
          EXECUTE UnAssign FROM assigned_services
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).

      " Pass any unassignment messages to the UI
      /plce/cl_base_misc=>check_response(
        EXPORTING is_response = srvc_unassign_reported
        CHANGING  ct_messages = reported-%other ).
    ENDIF.

    " 3. Process the Storno for each selected service
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      " Extract the input from your UI Popup
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

      TRY.
          " Call Reusable Waste Order Logic
          zcl_wr_waste_order_api=>cancel_waste_order_item(
            iv_pobjnr      = <ls_service>-ReferenceInternalId
            iv_reason_code = ls_param-definiertegrund
            iv_reason_text = ls_param-stornogrund
          ).

          " Add to Bulk Update tables instead of modifying inside the loop
          " NOTE: Replace 'CANC' with your actual constant for Cancelled/Storno
          APPEND VALUE #( %tky = <ls_service>-%tky
                          ServiceStatus = 'CANC' ) TO lt_service_update.

          APPEND VALUE #( %tky = <ls_service>-%tky
                          %target = VALUE #( ( %cid = |HIST_{ sy-tabix }| 
                                               ServiceStatus = 'CANC' ) ) ) TO lt_history_create.
          
          " Success Message
          APPEND NEW /plce/cx_pd_exception( 
                       severity = if_abap_behv_message=>severity-success
                       textid   = /plce/cx_pd_exception=>service_assigned " <-- Replace with a Storno success textid
                       serviceid = <ls_service>-ServiceId ) TO reported-%other.

        CATCH cx_eewa_base INTO DATA(lo_eewa_ex).
          " Safely map the legacy IS-U Exception to a RAP Message
          APPEND VALUE #( %tky = <ls_service>-%tky ) TO failed-service.
          
          APPEND VALUE #( %tky = <ls_service>-%tky 
                          %msg = new_message_with_text( 
                                   severity = if_abap_behv_message=>severity-error
                                   text     = lo_eewa_ex->get_text( ) ) 
                        ) TO reported-service.
      ENDTRY.
    ENDLOOP.

    " 4. Bulk Modify Entities (Performance Optimization)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
          UPDATE FIELDS ( ServiceStatus )
          WITH lt_service_update
          CREATE BY \_StatusHistory 
          SET FIELDS WITH lt_history_create.
    ENDIF.

    " 5. Return the updated data to the UI
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.
