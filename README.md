METHOD execute_cancel.
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
    lo_bo_item->dataref->conftype1 = iv_reason_code.
    
    " Append the text without overwriting existing comments
    IF lo_bo_item->dataref->text IS INITIAL.
      lo_bo_item->dataref->text = iv_reason_text.
    ELSE.
      lo_bo_item->dataref->text = lo_bo_item->dataref->text && ` ` && iv_reason_text.
    ENDIF.

    " 4. Perform the Negative Confirmation
    " (Removed zzv_auto_confirm as it is not defined in your BO class)
    TRY.
        " Standard Item BO methods to check and execute the confirmation
        lo_bo_item->check_book_confirm_pos( ).
        lo_bo_item->book_confirm_pos( ).
        
        " 5. Save using the inherited SAVE_BO and unlock
        save_bo( lo_bo_item ).
        lo_bo_item->unlock_for_edit( ). 
        
      CATCH cx_eewa_base INTO DATA(lex).
        " Ensure we unlock before passing the error up to Fiori
        lo_bo_item->unlock_for_edit( ).
        RAISE EXCEPTION lex.
    ENDTRY.

  ENDMETHOD.
