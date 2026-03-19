CLASS zcl_wr_waste_order_api DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    " Reusable method for Negative Confirmation / Storno
    CLASS-METHODS cancel_waste_order_item
      IMPORTING
        iv_pobjnr      TYPE pobjnr " From Service-ReferenceInternalId
        iv_reason_code TYPE string " Popup: definiertegrund
        iv_reason_text TYPE string " Popup: stornogrund
      RAISING
        cx_eewa_base. " Propagates IS-U BO exceptions to the caller
ENDCLASS.

CLASS zcl_wr_waste_order_api IMPLEMENTATION.

  METHOD cancel_waste_order_item.
    DATA: ls_item_key TYPE ewa_order_object_ikey,
          lo_bo_base  TYPE REF TO cl_eewa_bo_base,
          lo_bo_item  TYPE REF TO zcl_wr_eewa_bo_wdorderitem.

    " 1. Find the Order Item keys based on POBJNR
    SELECT SINGLE ordernr, order_laufnr
      FROM ewa_order_object
      INTO @ls_item_key
      WHERE pobjnr = @iv_pobjnr.

    CHECK sy-subrc = 0.

    " 2. Instantiate the IS-U Waste Order Item BO directly (Option 2)
    cl_eewa_bo_base=>get_instance(
      EXPORTING
        par_objtype = cl_eewa_bo_wdorderitem=>cot_wdorderitem
        par_key     = ls_item_key
      IMPORTING
        par_ref     = lo_bo_base 
    ).

    " Cast to your custom BO class to access your specific methods
    lo_bo_item ?= lo_bo_base.

    " 3. Lock the Business Object for editing
    lo_bo_item->enqueue( ).

    " 4. Apply the Storno Updates to the BO Data Reference
    lo_bo_item->dataref->conftype1 = iv_reason_code.
    
    " Append the text without overwriting existing comments
    IF lo_bo_item->dataref->text IS INITIAL.
      lo_bo_item->dataref->text = iv_reason_text.
    ELSE.
      lo_bo_item->dataref->text = lo_bo_item->dataref->text && ` ` && iv_reason_text.
    ENDIF.

    " Notify the BO framework that the item data has been modified
    lo_bo_item->item_changed( par_detailindex = lo_bo_item->dataref->detail_index ).

    " 5. Perform the Negative Confirmation
    " Temporarily enable your custom auto-confirm flag
    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_true.
    
    TRY.
        lo_bo_item->check_book_confirm_pos( ).
        lo_bo_item->book_confirm_pos( ).
        
        " 6. Save and Unlock
        lo_bo_item->save( ).
        
        " Use unlock_for_edit() as seen in your custom code (or dequeue() if standard)
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
