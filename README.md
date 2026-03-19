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
        cx_eewa_base. " Propagate IS-U exceptions to the caller
ENDCLASS.

CLASS zcl_wr_waste_order_api IMPLEMENTATION.

  METHOD cancel_waste_order_item.
    DATA: ls_item_key TYPE ewa_order_object_ikey,
          lo_bo_item  TYPE REF TO zcl_wr_eewa_bo_wdorderitem,
          lo_factory  TYPE REF TO cl_eewa_bo_factory.

    " 1. Find the Order Item keys based on POBJNR
    SELECT SINGLE ordernr, order_laufnr
      FROM ewa_order_object
      INTO @ls_item_key
      WHERE pobjnr = @iv_pobjnr.

    CHECK sy-subrc = 0.

    " 2. Instantiate the IS-U Waste Order Item BO
    " We use the standard factory approach to get the BO with a lock
    cl_eewa_bo_factory=>get_instance( IMPORTING par_ref = lo_factory ).
    
    lo_bo_item ?= lo_factory->getbo(
        par_objtype = cl_eewa_bo_wdorderitem=>cot_wdorderitem
        par_key     = ls_item_key
        par_lock    = 'X' ).

    " 3. Apply the Storno Updates to the BO Data Reference
    " Ensure we don't overwrite existing text completely, just append or set
    lo_bo_item->dataref->conftype1 = iv_reason_code. " e.g. "Storno durch Dispo" 
    
    IF lo_bo_item->dataref->text IS INITIAL.
      lo_bo_item->dataref->text = iv_reason_text.
    ELSE.
      lo_bo_item->dataref->text = lo_bo_item->dataref->text && ` ` && iv_reason_text.
    ENDIF.

    " Let the BO know the item was changed
    lo_bo_item->item_changed( par_detailindex = lo_bo_item->dataref->detail_index ).

    " 4. Perform the Negative Confirmation 
    " Reusing your custom logic from ZCL_PLCP_TA_WA_ORDER_RSLT
    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_true.
    TRY.
        lo_bo_item->check_book_confirm_pos( ).
        lo_bo_item->book_confirm_pos( ).
        
        " Save and unlock
        lo_factory->save_bo( lo_bo_item ).
        lo_bo_item->unlock_for_edit( ).
        
      CATCH cx_eewa_base INTO DATA(lex).
        " Ensure reset on failure and unlock
        zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_false.
        lo_bo_item->unlock_for_edit( ).
        RAISE EXCEPTION lex.
    ENDTRY.

    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_false.

  ENDMETHOD.

ENDCLASS.
