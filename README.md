METHOD terminateservice.
    DATA: assigned_services TYPE TABLE FOR ACTION IMPORT /PLCE/R_PDTour\\ServiceAssignment~UnAssign,
          lt_service_update TYPE TABLE FOR UPDATE /PLCE/R_PDService,
          lt_history_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_StatusHistory,
          lt_return         TYPE bapiret2_t.

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
      CLEAR lt_return.

      " -------------------------------------------------------------------------
      " 4. THE BREAKOUT: Call the RFC to bypass the RAP strict mode dump!
      " -------------------------------------------------------------------------
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-ReferenceInternalId )
          iv_reason_predefined = CONV char255( ls_param-definiertegrund )
          iv_reason_text       = CONV string( ls_param-stornogrund )
        IMPORTING
          et_return            = lt_return.

      " 5. Evaluate the RFC result
      IF lt_return IS INITIAL.
        " --- SUCCESS ---
        " Add to Bulk Update tables (Replace 'CANC' with your actual constant for Cancelled)
        APPEND VALUE #( %tky = <ls_service>-%tky
                        ServiceStatus = 'CANC' ) TO lt_service_update.

        APPEND VALUE #( %tky = <ls_service>-%tky
                        %target = VALUE #( ( %cid = |HIST_{ sy-tabix }|
                                             ServiceStatus = 'CANC' ) ) ) TO lt_history_create.

        " Success Message for the UI
        APPEND NEW /plce/cx_pd_exception(
                     severity  = if_abap_behv_message=>severity-success
                     textid    = /plce/cx_pd_exception=>service_assigned " Consider adding a specific Storno success textid
                     serviceid = <ls_service>-ServiceId ) TO reported-%other.
                     
      ELSE.
        " --- FAILURE ---
        " The legacy IS-U BO rejected the Storno. Map the errors to the Fiori UI.
        APPEND VALUE #( %tky = <ls_service>-%tky ) TO failed-service.

        LOOP AT lt_return INTO DATA(ls_ret).
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message_with_text(
                                   severity = if_abap_behv_message=>severity-error
                                   text     = CONV #( ls_ret-message ) )
                        ) TO reported-service.
        ENDLOOP.
      ENDIF.
    ENDLOOP.

    " 6. Bulk Modify RAP Entities (Performance Optimization)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
          UPDATE FIELDS ( ServiceStatus )
          WITH lt_service_update
          CREATE BY \_StatusHistory
          SET FIELDS WITH lt_history_create.
    ENDIF.

    " 7. Return the updated data to the UI
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.
