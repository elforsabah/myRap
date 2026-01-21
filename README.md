FUNCTION z_create_lanf_from_bms.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IV_CONTRACT_VBELN) TYPE VBELN_VA
*"     VALUE(IV_LEISTUNGSDATUM) TYPE FBUDA
*"     VALUE(IV_BSTKD) TYPE BSTKD OPTIONAL
*"     VALUE(IT_POSITIONS) TYPE ztt_lanf_positions  " Table type: MATNR, ZMENG, ZIEME
*"  EXPORTING
*"     VALUE(EV_LANF_VBELN) TYPE VBELN_VA
*"     VALUE(ET_MESSAGES) TYPE bapiret2_t
*"----------------------------------------------------------------------

  DATA: ls_header       TYPE bapisdhd1,
        lt_items        TYPE TABLE OF bapisditm,
        ls_item         TYPE bapisditm,
        lt_itemsx       TYPE TABLE OF bapisditmx,
        ls_itemx        TYPE bapisditmx,
        lt_partners     TYPE TABLE OF bapiparnr,
        lt_schedules    TYPE TABLE OF bapischdl,
        ls_schedule     TYPE bapischdl,
        lt_schedulesx   TYPE TABLE OF bapischdlx,
        ls_schedulex    TYPE bapischdlx,
        lt_return       TYPE TABLE OF bapiret2,
        lv_vbeln        TYPE vbeln_va.

  " Step 1: Validate input and fetch contract header data
  SELECT SINGLE * FROM vbak INTO @DATA(ls_vbak)
    WHERE vbeln = @iv_contract_vbeln.
  IF sy-subrc <> 0.
    APPEND VALUE #(type = 'E' id = 'ZMSG' number = '001' message = 'Contract not found') TO et_messages.
    RETURN.
  ENDIF.

  " Fetch contract items to map MATNR to POSNR (assumption: unique per MATNR)
  SELECT * FROM vbap INTO TABLE @DATA(lt_vbap)
    WHERE vbeln = @iv_contract_vbeln.

  " Step 2: Populate BAPI header
  ls_header-doc_type   = 'DR'.  " Debit memo request order type (adjust if custom)
  ls_header-sales_org  = ls_vbak-vkorg.
  ls_header-distr_chan = ls_vbak-vtweg.
  ls_header-division   = ls_vbak-spart.
  ls_header-purch_no_c = iv_bstkd.  " Kundenreferenz
  ls_header-req_date_h = iv_leistungsdatum.  " Leistungsdatum (header requested delivery date)

  " Step 3: Populate partners (copy from contract, e.g., sold-to)
  APPEND VALUE #(partn_role = 'AG' partn_numb = ls_vbak-kunnr) TO lt_partners.  " Sold-to
  " Add ship-to, etc., if needed via BAPI_CONTRACT_GETDETAIL for full details

  " Step 4: Populate items and schedules
  LOOP AT it_positions INTO DATA(ls_pos) WHERE zmeng > 0.  " Drop zero quantities
    " Find unique POSNR for MATNR in contract
    READ TABLE lt_vbap INTO DATA(ls_vbap_pos) WITH KEY matnr = ls_pos-matnr.
    IF sy-subrc <> 0.
      APPEND VALUE #(type = 'W' id = 'ZMSG' number = '002' message = |No contract position for MATNR {ls_pos-matnr}|) TO et_messages.
      CONTINUE.
    ENDIF.

    " Item data
    CLEAR ls_item.
    ls_item-itm_number = sy-tabix * 10.  " e.g., 000010, 000020
    ls_item-material   = ls_pos-matnr.
    ls_item-target_qty = ls_pos-zmeng.
    ls_item-target_qu  = ls_pos-zieme.
    ls_item-ref_doc    = iv_contract_vbeln.  " Reference contract
    ls_item-ref_doc_it = ls_vbap_pos-posnr.  " Reference contract item
    ls_item-ref_doc_ca = 'G'.  " Category: Contract
    APPEND ls_item TO lt_items.

    " Item update flags
    CLEAR ls_itemx.
    ls_itemx-itm_number = ls_item-itm_number.
    ls_itemx-updateflag = 'I'.
    ls_itemx-material   = 'X'.
    ls_itemx-target_qty = 'X'.
    ls_itemx-target_qu  = 'X'.
    ls_itemx-ref_doc    = 'X'.
    ls_itemx-ref_doc_it = 'X'.
    ls_itemx-ref_doc_ca = 'X'.
    APPEND ls_itemx TO lt_itemsx.

    " Schedule line (for Leistungsdatum)
    CLEAR ls_schedule.
    ls_schedule-itm_number = ls_item-itm_number.
    ls_schedule-sched_line = '0001'.
    ls_schedule-req_date   = iv_leistungsdatum.  " KETDAT/FBUDA
    ls_schedule-req_qty    = ls_pos-zmeng.
    APPEND ls_schedule TO lt_schedules.

    CLEAR ls_schedulex.
    ls_schedulex-itm_number = ls_schedule-itm_number.
    ls_schedulex-sched_line = ls_schedule-sched_line.
    ls_schedulex-updateflag = 'I'.
    ls_schedulex-req_date   = 'X'.
    ls_schedulex-req_qty    = 'X'.
    APPEND ls_schedulex TO lt_schedulesx.
  ENDLOOP.

  IF lt_items IS INITIAL.
    APPEND VALUE #(type = 'E' id = 'ZMSG' number = '003' message = 'No valid positions') TO et_messages.
    RETURN.
  ENDIF.

  " Step 5: Call BAPI to create
  CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
    EXPORTING
      order_header_in   = ls_header
    IMPORTING
      salesdocument     = lv_vbeln
    TABLES
      return            = lt_return
      order_items_in    = lt_items
      order_items_inx   = lt_itemsx
      order_partners    = lt_partners
      order_schedules_in = lt_schedules
      order_schedules_inx = lt_schedulesx.

  " Step 6: Commit and handle return
  READ TABLE lt_return TRANSPORTING NO FIELDS WITH KEY type = 'E'.
  IF sy-subrc = 0.
    CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
    et_messages = lt_return.
  ELSE.
    CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = 'X'.
    ev_lanf_vbeln = lv_vbeln.
    et_messages = lt_return.  " Success messages
  ENDIF.

ENDFUNCTION.
