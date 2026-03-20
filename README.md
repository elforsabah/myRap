METHOD terminateservice.
    DATA: lt_unassign_keys  TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour\\serviceassignment~unassign,
          lt_service_update TYPE TABLE FOR UPDATE /plce/r_pdservice,
          lt_return         TYPE bapiret2_t.

    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid ReferenceId servicestatus serviceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.
      CLEAR lt_return.

      " 1. Do the Waste Order Storno in the Backend
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-referenceinternalid )
          iv_reason_predefined = CONV char8( ls_param-definiertegrund )
          iv_reason_text       = CONV string( ls_param-stornogrund )
        IMPORTING
          et_return            = lt_return.

      IF lt_return IS INITIAL.
        " --- SUCCESS ---

        " A. Update Status to '01' to keep Fiori happy for the next 3 seconds
        APPEND VALUE #( %tky = <ls_service>-%tky
                        servicestatus = '01' ) TO lt_service_update.

        " B. TRIGGER THE HARD DELETE IN THE BACKGROUND!
        " "IN BACKGROUND TASK" means this RFC won't start until the RAP COMMIT finishes.
        CALL FUNCTION 'Z_WR_DELAYED_HARD_DELETE' IN BACKGROUND TASK
          EXPORTING
            iv_serviceuuid = <ls_service>-serviceuuid.

        " C. Prepare Tour Unassignment 
        SELECT SINGLE asgmt~serviceuuid, asgmt~touruuid
          FROM /plce/r_pdtourserviceasgmt AS asgmt
          WHERE asgmt~serviceuuid = @<ls_service>-serviceuuid
          INTO @DATA(ls_unassign).

        IF sy-subrc = 0.
          APPEND VALUE #( %key-touruuid    = ls_unassign-touruuid
                          %key-serviceuuid = ls_unassign-serviceuuid ) TO lt_unassign_keys.
        ENDIF.

        " D. Success Message
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                            number   = '009'
                                            severity = if_abap_behv_message=>severity-information
                                            v1       = |{ <ls_service>-ReferenceId ALPHA = OUT }|  ) 
                      ) TO reported-service.

      ELSE.
        " --- FAILURE ---
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

    " 2. Bulk Modify RAP Entities (Just the temporary status update)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          UPDATE FIELDS ( servicestatus ) WITH lt_service_update.
    ENDIF.

    " 3. Bulk Execute Tour Unassignment 
    IF lt_unassign_keys IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdtour
        ENTITY serviceassignment
          EXECUTE unassign FROM lt_unassign_keys
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).
    ENDIF.

    " 4. Return the updated data to the UI 
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.
