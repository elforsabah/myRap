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

      " 1. Do the Waste Order Storno/Delete in the Backend
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-referenceinternalid )
          iv_reason_predefined = CONV char8( ls_param-definiertegrund )
          iv_reason_text       = CONV string( ls_param-stornogrund )
          iv_serviceuuid       = <ls_service>-serviceuuid
        IMPORTING
          et_return            = lt_return.

      IF lt_return IS INITIAL.
        " --- SUCCESS ---

        " A. Update Status to '01' to keep Fiori happy for the next 3 seconds
        APPEND VALUE #( %tky = <ls_service>-%tky
                        servicestatus = '01' ) TO lt_service_update.

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

FUNCTION Z_WR_DELAYED_HARD_DELETE_RFC.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  IMPORTING
*"     VALUE(IV_SERVICEUUID) TYPE  /PLCE/PDSERVICE_UUID
*"----------------------------------------------------------------------
 " 1. Wait for 3 seconds.

  WAIT UP TO 3 SECONDS.
  DELETE FROM /plce/tpdsrv WHERE service_uuid = iv_serviceuuid.
  DELETE FROM /plce/tpdsrvtsk WHERE service_uuid = iv_serviceuuid.
  COMMIT WORK.

ENDFUNCTION



  FUNCTION z_wr_cancel_waste_order_rfc.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  IMPORTING
*"     VALUE(IV_POBJNR) TYPE  CHAR30
*"     VALUE(IV_REASON_PREDEFINED) TYPE  CHAR8
*"     VALUE(IV_REASON_TEXT) TYPE  STRING
*"     VALUE(IV_SERVICEUUID) TYPE  /PLCE/PDSERVICE_UUID OPTIONAL
*"  EXPORTING
*"     VALUE(ET_RETURN) TYPE  BAPIRET2_T
*"----------------------------------------------------------------------


  TRY.
      zcl_wr_pd_order_cancelation=>cancel_waste_order_item(
        iv_pobjnr            = iv_pobjnr
        iv_reason_predefined = iv_reason_predefined
        iv_reason_text       = iv_reason_text
        iv_serviceuuid = IV_SERVICEUUID
      ).

      " Crucial: Commit the isolated RFC session so the DB update is written!
      COMMIT WORK AND WAIT.

    CATCH cx_eewa_base INTO DATA(lx_eewa).
      ROLLBACK WORK.
**      " Pass the error back so the RAP Fiori app can display it
      APPEND VALUE #( type    = 'E'
*                      id      = lx_eewa->if_message~t100key-msgid
*                      number  = lx_eewa->if_t100_message~t100key-msgno
                      message = lx_eewa->get_text( ) ) TO et_return.
  ENDTRY.

ENDFUNCTION.


  METHOD CANCEL_WASTE_ORDER_ITEM.
    " 1. Instantiate this class to access the inherited instance methods
    DATA(lo_api) = NEW zcl_wr_pd_order_cancelation( ).
    " 2. Call the instance method
    lo_api->execute_cancel(
      iv_pobjnr      = iv_pobjnr
      IV_REASON_PREDEFINED = iv_reason_predefined
      iv_reason_text = iv_reason_text
      iv_serviceuuid = iv_serviceuuid
    ).
  ENDMETHOD.

   METHOD EXECUTE_CANCEL.
    DATA: ls_item_key TYPE ewa_order_object_ikey,
          lo_bo_item  TYPE REF TO zcl_wr_eewa_bo_wdorderitem.

    " 1. Find the Order Item keys based on POBJNR
    SELECT SINGLE ordernr, order_laufnr
      FROM ewa_order_object
      INTO @ls_item_key
      WHERE pobjnr = @iv_pobjnr.

    CHECK sy-subrc = 0.

    " 2. Instantiate the IS-U Waste Order Item BO using inherited GETBO
    lo_bo_item ?= getbo(
        par_objtype = cl_eewa_bo_wdorderitem=>cot_wdorderitem
        par_key     = ls_item_key
        par_lock    = 'X' ).

    " 3. Apply the Storno Updates to the BO Data Reference
    lo_bo_item->dataref->conftype1 = iv_reason_predefined.  " Vordefinierte Grund
    lo_bo_item->dataref->text = iv_reason_text." Gegebene Grund
    lo_bo_item->dataref->eorder_status = 03. "Negative Rückmeldung
    " Append the text without overwriting existing comments
    TRY.
        " Standard Item BO methods to check and execute the confirmation
        lo_bo_item->check_book_confirm_neg( ). "Negative Rückmeldung
        lo_bo_item->book_confirm_neg( ).       "Negative Rückmeldung



        " -----------------------------------------------------------------
        " TRIGGER BACKGROUND TASK HERE!
        " This is completely hidden from RAP and perfectly safe.
        " -----------------------------------------------------------------
        IF iv_serviceuuid IS NOT INITIAL.
          CALL FUNCTION 'Z_WR_DELAYED_HARD_DELETE' IN BACKGROUND TASK
            EXPORTING
              iv_serviceuuid = iv_serviceuuid.
        ENDIF.
        " -----------------------------------------------------------------

       " 5. Save using the inherited SAVE_BO and unlock
        save_bo( lo_bo_item ).
        lo_bo_item->unlock_for_edit( ).

      CATCH cx_eewa_base INTO DATA(lex).
        " Ensure we unlock before passing the error up to Fiori
        lo_bo_item->unlock_for_edit( ).
        RAISE EXCEPTION lex.
    ENDTRY.

  ENDMETHOD.
