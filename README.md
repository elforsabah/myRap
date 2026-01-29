METHOD createlanf.

  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "your target doc type

  DATA:
    ls_key          LIKE LINE OF keys,
    ls_input        TYPE zi_lanf_create_input,  "adjust to your %param type if different
    lv_contract     TYPE vbeln_va,
    lv_new_vbeln    TYPE vbeln_va,
    ls_contract_hdr TYPE vbak,
    lt_return       TYPE bapiret2_tab.

  "BAPI structures (DAT2)
  DATA:
    ls_head_in   TYPE bapisdhd1,
    ls_head_inx  TYPE bapisdhd1x,
    ls_logic     TYPE bapisdls,
    lt_items_in  TYPE bapisditm_tt,
    lt_items_inx TYPE bapisditmx_tt,
    lt_sched_in  TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
    lt_sched_inx TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY,
    lt_partners  TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY.

  DATA:
    ls_item_in   TYPE bapisditm,
    ls_item_inx  TYPE bapisditmx,
    ls_sch_in    TYPE bapischdl,
    ls_sch_inx   TYPE bapischdlx.

  "-----------------------------
  " 1) Read input
  "-----------------------------
  READ TABLE keys INTO ls_key INDEX 1.
  ls_input = ls_key-%param.

  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "-----------------------------
  " 2) Read contract header
  "-----------------------------
  SELECT SINGLE *
    FROM vbak
    WHERE vbeln = @lv_contract
    INTO @ls_contract_hdr.

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |Contract { lv_contract } not found|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 3) Header (DAT2)
  "-----------------------------
  CLEAR: ls_head_in, ls_head_inx.

  ls_head_in-doc_type    = gc_auart.
  ls_head_in-sales_org   = ls_contract_hdr-vkorg.
  ls_head_in-distr_chan  = ls_contract_hdr-vtweg.
  ls_head_in-division    = ls_contract_hdr-spart.

  "Reference to contract (so copy-control/pricing can work)
  ls_head_in-ref_doc     = lv_contract.
  ls_head_in-refdoc_cat  = 'G'.

  ls_head_in-req_date_h  = ls_input-deliverydate.
  ls_head_in-purch_no_c  = ls_input-customerref.

  ls_head_inx-updateflag = 'I'.
  ls_head_inx-doc_type   = 'X'.
  ls_head_inx-sales_org  = 'X'.
  ls_head_inx-distr_chan = 'X'.
  ls_head_inx-division   = 'X'.
  ls_head_inx-ref_doc    = 'X'.
  ls_head_inx-refdoc_cat = 'X'.
  ls_head_inx-req_date_h = 'X'.
  ls_head_inx-purch_no_c = 'X'.

  "-----------------------------
  " 4) Partners from contract
  "-----------------------------
  SELECT parvw, kunnr
    FROM vbpa
    WHERE vbeln = @lv_contract
      AND kunnr <> ''
      AND parvw IN ('AG','WE','RE','RG','PY')
    INTO TABLE @DATA(lt_vbpa).

  lt_partners = VALUE #(
    FOR p IN lt_vbpa
    ( partn_role = p-parvw
      partn_numb = p-kunnr ) ).

  "-----------------------------
  " 5) Map input positions -> contract items (POSNR by MATNR)
  "-----------------------------
  SELECT posnr, matnr, werks
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_ctr_items).

  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    DATA lv_matnr TYPE matnr.
    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_ctr_items INTO DATA(ls_ctr)
      WITH KEY matnr = lv_matnr.

    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    CLEAR: ls_item_in, ls_item_inx, ls_sch_in, ls_sch_inx.

    "Item (important: set TARGET_QTY so it is not incomplete)
    ls_item_in-itm_number = ls_ctr-posnr.
    ls_item_in-material   = lv_matnr.
    ls_item_in-target_qty = ls_pos-menge.
    ls_item_in-target_qu  = ls_pos-meins.

    "Reference to contract item
    ls_item_in-ref_doc    = lv_contract.
    ls_item_in-ref_doc_it = ls_ctr-posnr.
    ls_item_in-ref_doc_ca = 'G'.

    "Plant (often required)
    IF ls_ctr-werks IS NOT INITIAL.
      ls_item_in-plant = ls_ctr-werks.
    ENDIF.

    APPEND ls_item_in TO lt_items_in.

    "Item X
    ls_item_inx-itm_number = ls_ctr-posnr.
    ls_item_inx-updateflag = 'I'.
    ls_item_inx-material   = 'X'.
    ls_item_inx-target_qty = 'X'.
    ls_item_inx-target_qu  = 'X'.
    ls_item_inx-ref_doc    = 'X'.
    ls_item_inx-ref_doc_it = 'X'.
    ls_item_inx-ref_doc_ca = 'X'.
    IF ls_item_in-plant IS NOT INITIAL.
      ls_item_inx-plant = 'X'.
    ENDIF.

    APPEND ls_item_inx TO lt_items_inx.

    "Schedule line (helps ensure target qty is consistent)
    ls_sch_in-itm_number = ls_ctr-posnr.
    ls_sch_in-sched_line = '0001'.
    ls_sch_in-req_qty    = ls_pos-menge.
    ls_sch_in-req_date   = ls_input-deliverydate.
    APPEND ls_sch_in TO lt_sched_in.

    ls_sch_inx-itm_number = ls_ctr-posnr.
    ls_sch_inx-sched_line = '0001'.
    ls_sch_inx-updateflag = 'I'.
    ls_sch_inx-req_qty    = 'X'.
    ls_sch_inx-req_date   = 'X'.
    APPEND ls_sch_inx TO lt_sched_inx.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 6) Logic switch (pricing/conditions handling)
  "-----------------------------
  CLEAR ls_logic.
  ls_logic-cond_handl = 'X'.  "condition handling (useful for pricing/conditions) :contentReference[oaicite:2]{index=2}

  "-----------------------------
  " 7) Create (no COMMIT here in RAP)
  "-----------------------------
  CLEAR: lt_return, lv_new_vbeln.

  CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
    EXPORTING
      order_header_in      = ls_head_in
      order_header_inx     = ls_head_inx
      logic_switch         = ls_logic
    IMPORTING
      salesdocument        = lv_new_vbeln
    TABLES
      return               = lt_return
      order_items_in       = lt_items_in
      order_items_inx      = lt_items_inx
      order_schedules_in   = lt_sched_in
      order_schedules_inx  = lt_sched_inx
      order_partners       = lt_partners.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_err) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id       = ls_err-id
        number   = ls_err-number
        v1       = ls_err-message_v1
        v2       = ls_err-message_v2
        v3       = ls_err-message_v3
        v4       = ls_err-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  IF lv_new_vbeln IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |BAPI returned no document number|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Success
  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_vbeln
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (CREATEFROMDAT2)' ) )
    ) )
  ).

ENDMETHOD.
