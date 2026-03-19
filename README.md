METHOD terminateService.
    DATA: assigned_services TYPE TABLE FOR ACTION IMPORT /PLCE/R_PDTour\\ServiceAssignment~UnAssign.

    " 1. Read the service to get POBJNR (ReferenceInternalId)
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      FIELDS ( ServiceUUID ReferenceInternalId ServiceStatus ServiceId )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " 2. Unassign from Tour (if it is currently planned)
    " We borrow this exact logic from your UnassignOnDelete method
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
          " 4. Call Reusable Waste Order Logic
          zcl_wr_waste_order_api=>cancel_waste_order_item(
            iv_pobjnr      = <ls_service>-ReferenceInternalId
            iv_reason_code = ls_param-definiertegrund
            iv_reason_text = ls_param-stornogrund
          ).

          " 5. Set Service Status to Cancelled to remove from active planning
          " NOTE: Replace 'CANC' with your actual constant for Cancelled/Storno
          MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
            ENTITY Service
              UPDATE FIELDS ( ServiceStatus )
              WITH VALUE #( ( %tky = <ls_service>-%tky
                              ServiceStatus = 'CANC' ) ) 
              CREATE BY \_StatusHistory AUTO FILL CID
              SET FIELDS WITH VALUE #( ( %tky = <ls_service>-%tky
                                         %target = VALUE #( ( ServiceStatus = 'CANC' ) ) ) ).
          
          " Success Message
          APPEND NEW /plce/cx_pd_exception( 
                       severity = if_abap_behv_message=>severity-success
                       textid   = /plce/cx_pd_exception=>service_assigned " Replace with a Storno success textid
                       serviceid = <ls_service>-ServiceId ) TO reported-%other.

        CATCH cx_eewa_base INTO DATA(lo_eewa_ex).
          " If the legacy IS-U BO throws an error, catch it and show it in Fiori
          APPEND VALUE #( %tky = <ls_service>-%tky ) TO failed-service.
          APPEND VALUE #( %tky = <ls_service>-%tky 
                          %msg = lo_eewa_ex ) TO reported-service.
      ENDTRY.
    ENDLOOP.

    " 6. Return the updated data to the UI
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.
