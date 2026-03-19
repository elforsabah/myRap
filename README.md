CLASS zcl_wr_waste_order_api DEFINITION
  PUBLIC
  INHERITING FROM /plcp/cl_ta_wa_order_rslt
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    " 1. Define a structure and static table to hold requests in memory
    TYPES: BEGIN OF ty_cancel_req,
             pobjnr      TYPE pobjnr,
             reason_code TYPE string,
             reason_text TYPE string,
           END OF ty_cancel_req.
           
    CLASS-DATA: gt_cancel_requests TYPE TABLE OF ty_cancel_req.

    " 2. Called from your RAP Action (Interaction Phase) - NO DB UPDATES HERE!
    CLASS-METHODS register_cancel_request
      IMPORTING
        iv_pobjnr      TYPE pobjnr
        iv_reason_code TYPE string
        iv_reason_text TYPE string.

    " 3. Called from the RAP Saver Class (Save Phase) - DB UPDATES ALLOWED!
    CLASS-METHODS execute_deferred_cancels.

  PRIVATE SECTION.
    METHODS execute_cancel
      IMPORTING
        is_req TYPE ty_cancel_req
      RAISING
        cx_eewa_base.
ENDCLASS.

CLASS zcl_wr_waste_order_api IMPLEMENTATION.

  METHOD register_cancel_request.
    " Store the request in memory for later. This avoids the RAP Dump!
    APPEND VALUE #( pobjnr      = iv_pobjnr
                    reason_code = iv_reason_code
                    reason_text = iv_reason_text ) TO gt_cancel_requests.
  ENDMETHOD.

  METHOD execute_deferred_cancels.
    " Loop through all buffered requests and process them now that we are in the Save Phase
    DATA(lo_api) = NEW zcl_wr_waste_order_api( ).
    
    LOOP AT gt_cancel_requests INTO DATA(ls_req).
      TRY.
          lo_api->execute_cancel( ls_req ).
        CATCH cx_eewa_base.
          " In the Save phase, we can't easily send messages back to the UI, 
          " so we catch and ignore/log the error to prevent crashes.
      ENDTRY.
    ENDLOOP.
    
    CLEAR gt_cancel_requests. " Empty the buffer after saving
  ENDMETHOD.

  METHOD execute_cancel.
    DATA: ls_item_key TYPE ewa_order_object_ikey,
          lo_bo_item  TYPE REF TO zcl_wr_eewa_bo_wdorderitem.

    SELECT SINGLE ordernr, order_laufnr FROM ewa_order_object INTO @ls_item_key WHERE pobjnr = @is_req-pobjnr.
    CHECK sy-subrc = 0.

    lo_bo_item ?= getbo( par_objtype = cl_eewa_bo_wdorderitem=>cot_wdorderitem
                         par_key     = ls_item_key
                         par_lock    = 'X' ).

    lo_bo_item->dataref->conftype1 = is_req-reason_code.
    
    IF lo_bo_item->dataref->text IS INITIAL.
      lo_bo_item->dataref->text = is_req-reason_text.
    ELSE.
      lo_bo_item->dataref->text = lo_bo_item->dataref->text && ` ` && is_req-reason_text.
    ENDIF.

    " Provide the minimum required data for a Storno
    IF lo_bo_item->dataref->actual_date IS INITIAL.
      lo_bo_item->dataref->actual_date = sy-datum.
    ENDIF.
    IF lo_bo_item->dataref->actual_time IS INITIAL.
      lo_bo_item->dataref->actual_time = sy-uzeit.
    ENDIF.
    lo_bo_item->dataref->beh_anzahl_rm = 0. 

    lo_bo_item->item_changed( par_detailindex = lo_bo_item->dataref->detail_index ).

    TRY.
        lo_bo_item->check_book_confirm_pos( ).
        lo_bo_item->book_confirm_pos( ).
        save_bo( lo_bo_item ). " <-- This is now safe because it runs in the Save Phase!
        lo_bo_item->unlock_for_edit( ). 
      CATCH cx_eewa_base INTO DATA(lex).
        lo_bo_item->unlock_for_edit( ).
        RAISE EXCEPTION lex.
    ENDTRY.
  ENDMETHOD.

ENDCLASS.
