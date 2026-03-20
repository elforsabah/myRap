METHOD terminateservice.
    DATA: lt_unassign_keys  TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour\\serviceassignment~unassign,
          lt_service_update TYPE TABLE FOR UPDATE /plce/r_pdservice,
          lt_history_create TYPE TABLE FOR CREATE /plce/r_pdservice\_statushistory,
          lt_return         TYPE bapiret2_t.

    " 1. Read the service to get POBJNR (ReferenceInternalId)
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid servicestatus serviceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " 2. Process the Storno for each selected service
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.
      CLEAR lt_return.

      " -------------------------------------------------------------------------
      " Call the RFC to bypass the RAP strict mode dump 
      " -------------------------------------------------------------------------
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-referenceinternalid )
          iv_reason_predefined = CONV char8( ls_param-definiertegrund )
          iv_reason_text       = CONV string( ls_param-stornogrund )
        IMPORTING
          et_return            = lt_return.

      " 3. Evaluate the RFC result
      IF lt_return IS INITIAL.
        " --- SUCCESS ---
        
        " A. Prepare Status Updates (Status 01)
        APPEND VALUE #( %tky = <ls_service>-%tky
                        servicestatus = '01' ) TO lt_service_update.

        APPEND VALUE #( %tky = <ls_service>-%tky
                        %target = VALUE #( ( %cid = |HIST_{ sy-tabix }|
                                             servicestatus = '01' ) ) ) TO lt_history_create.

        " B. Prepare Tour Unassignment (Only unassign if Storno succeeds!)
        SELECT SINGLE asgmt~serviceuuid, asgmt~touruuid
          FROM /plce/r_pdtourserviceasgmt AS asgmt
          WHERE asgmt~serviceuuid = @<ls_service>-serviceuuid
          INTO @DATA(ls_unassign).
          
        IF sy-subrc = 0.
          APPEND ls_unassign TO lt_unassign_keys.
        ENDIF.

        " C. SUCCESS MESSAGE 009: "Auftrag &1 wurde erfolgreich storniert"
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT' 
                                            number   = '009'
                                            severity = if_abap_behv_message=>severity-success
                                            v1       = <ls_service>-serviceid ) 
                      ) TO reported-service.

      ELSE.
        " --- FAILURE ---
        " The backend rejected the Storno (e.g. the WA19 error). 
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

    " 4. Bulk Modify RAP Entities (Status Updates)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          UPDATE FIELDS ( servicestatus )
          WITH lt_service_update
          CREATE BY \_statushistory
          SET FIELDS WITH lt_history_create.
    ENDIF.

    " 5. Bulk Execute Tour Unassignment (Only for successful Stornos)
    IF lt_unassign_keys IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdtour
        ENTITY serviceassignment
          EXECUTE unassign FROM lt_unassign_keys
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).

      /plce/cl_base_misc=>check_response(
        EXPORTING is_response = srvc_unassign_reported
        CHANGING  ct_messages = reported-%other ).
    ENDIF.

    " 6. Return the updated data to the UI
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.
