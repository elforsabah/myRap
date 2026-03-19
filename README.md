CLASS zcl_wr_waste_order_api DEFINITION
  PUBLIC
  INHERITING FROM /plcp/cl_ta_wa_order_rslt " <-- Inherit to access GETBO & SAVE_BO
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    " Static wrapper for easy calling from RAP / Interaction Center
    CLASS-METHODS cancel_waste_order_item
      IMPORTING
        iv_pobjnr      TYPE pobjnr
        iv_reason_code TYPE string
        iv_reason_text TYPE string
      RAISING
        cx_eewa_base.

  PRIVATE SECTION.
    " Instance method to actually execute the logic using inherited methods
    METHODS execute_cancel
      IMPORTING
        iv_pobjnr      TYPE pobjnr
        iv_reason_code TYPE string
        iv_reason_text TYPE string
      RAISING
        cx_eewa_base.
ENDCLASS.

CLASS zcl_wr_waste_order_api IMPLEMENTATION.

  METHOD cancel_waste_order_item.
    " 1. Instantiate this class to access the inherited instance methods
    DATA(lo_api) = NEW zcl_wr_waste_order_api( ).
    
    " 2. Call the instance method
    lo_api->execute_cancel(
      iv_pobjnr      = iv_pobjnr
      iv_reason_code = iv_reason_code
      iv_reason_text = iv_reason_text
    ).
  ENDMETHOD.

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

    " Notify the BO framework that the item data has been modified
    lo_bo_item->item_changed( par_detailindex = lo_bo_item->dataref->detail_index ).

    " 4. Perform the Negative Confirmation
    " Temporarily enable your custom auto-confirm flag
    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_true.
    
    TRY.
        lo_bo_item->check_book_confirm_pos( ).
        lo_bo_item->book_confirm_pos( ).
        
        " 5. Save using the inherited SAVE_BO and unlock
        save_bo( lo_bo_item ).
        lo_bo_item->unlock_for_edit( ). 
        
      CATCH cx_eewa_base INTO DATA(lex).
        " Always reset flag and unlock before passing the error up
        zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_false.
        lo_bo_item->unlock_for_edit( ).
        RAISE EXCEPTION lex.
    ENDTRY.

    " Reset flag on success
    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_false.

  ENDMETHOD.

ENDCLASS.
