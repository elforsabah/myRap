
METHOD createlanf.

  "=============================================================
  " CREATEFROMDAT2 approach (recommended)
  " Fixes M_/011 by aggregating input positions so each
  " contract item (POSNR) appears only once in BAPI tables.
  "=============================================================

  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "Target sales order type

  "-----------------------------
  " Input
  "-----------------------------
  DATA ls_key LIKE LINE OF keys.
  READ TABLE keys INTO ls_key INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA lv_contract TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "-----------------------------
  " Validate contract + get org data
  "-----------------------------
  DATA ls_contract_hdr TYPE vbak.
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
  " Read contract items (for mapping MATNR -> POSNR and plant)
  "-----------------------------
  TYPES: BEGIN OF ty_ctr_item,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           werks TYPE werks_d,
         END OF ty_ctr_item.

  DATA lt_ctr_items TYPE STANDARD TABLE OF ty_ctr_item WITH EMPTY KEY.

  SELECT posnr, matnr, werks
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @lt_ctr_items.

  IF lt_ctr_items IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |No items found in contract { lv_contract }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " Aggregate input positions by contract POSNR (fix M_/011)
  "-----------------------------
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,   "contract item posnr
           matnr TYPE matnr,      "normalized matnr
           werks TYPE werks_d,
           meins TYPE vrkme,
           qty   TYPE kwmeng,
         END OF ty_agg.

  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
  FIELD-SYMBOLS <agg> TYPE ty_agg.

  DATA lv_matnr TYPE matnr.

  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_ctr_items INTO DATA(ls_ctr) WITH KEY matnr = lv_matnr.
    IF sy-subrc <> 0 OR ls_ctr-posnr IS INITIAL.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
    IF sy-subrc = 0.
      "same contract item appears again -> sum qty (prevents duplicate ITM_NUMBER)
      IF <agg>-meins <> ls_pos-meins.
        APPEND VALUE #( %msg = new_message(
          id       = '00'
          number   = '001'
          v1       = |Same item { ls_ctr-posnr } has different UoM ({ <agg>-meins } vs { ls_pos-meins })|
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
        CONTINUE.
      ENDIF.
      <agg>-qty = <agg>-qty + ls_pos-menge.
    ELSE.
      INSERT VALUE ty_agg(
        posnr = ls_ctr-posnr
        matnr = lv_matnr
        werks = ls_ctr-werks
        meins = ls_pos-meins
        qty   = ls_pos-menge ) INTO TABLE lt_agg.
    ENDIF.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  IF lt_agg IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |No valid items found (all quantities <= 0 or mapping failed)|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " Partners from contract
  "-----------------------------
  DATA lt_partners TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY.

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
  " Header (DAT2)
  "-----------------------------
  DATA: ls_head_in  TYPE bapisdhd1,
        ls_head_inx TYPE bapisdhd1x.

  CLEAR: ls_head_in, ls_head_inx.

  ls_head_in-doc_type    = gc_auart.
  ls_head_in-sales_org   = ls_contract_hdr-vkorg.
  ls_head_in-distr_chan  = ls_contract_hdr-vtweg.
  ls_head_in-division    = ls_contract_hdr-spart.

  "reference to contract
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
  " Items + Schedules (ONE per POSNR)
  "-----------------------------
  DATA: lt_items_in  TYPE bapisditm_tt,
        lt_items_inx TYPE bapisditmx_tt,
        lt_sched_in  TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
        lt_sched_inx TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY.

  DATA: ls_item_in  TYPE bapisditm,
        ls_item_inx TYPE bapisditmx,
        ls_sch_in   TYPE bapischdl,
        ls_sch_inx  TYPE bapischdlx.

  CLEAR: lt_items_in, lt_items_inx, lt_sched_in, lt_sched_inx.

  LOOP AT lt_agg INTO DATA(ls_agg).

    CLEAR: ls_item_in, ls_item_inx, ls_sch_in, ls_sch_inx.

    "Item
    ls_item_in-itm_number = ls_agg-posnr.
    ls_item_in-material   = ls_agg-matnr.
    ls_item_in-target_qty = ls_agg-qty.
    ls_item_in-target_qu  = ls_agg-meins.

    ls_item_in-ref_doc    = lv_contract.
    ls_item_in-ref_doc_it = ls_agg-posnr.
    ls_item_in-ref_doc_ca = 'G'.

    IF ls_agg-werks IS NOT INITIAL.
      ls_item_in-plant = ls_agg-werks.
    ENDIF.

    APPEND ls_item_in TO lt_items_in.

    "Item X
    ls_item_inx-itm_number = ls_agg-posnr.
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

    "Schedule line (helps completeness & consistency)
    ls_sch_in-itm_number = ls_agg-posnr.
    ls_sch_in-sched_line = '0001'.
    ls_sch_in-req_qty    = ls_agg-qty.
    ls_sch_in-req_date   = ls_input-deliverydate.
    APPEND ls_sch_in TO lt_sched_in.

    ls_sch_inx-itm_number = ls_agg-posnr.
    ls_sch_inx-sched_line = '0001'.
    ls_sch_inx-updateflag = 'I'.
    ls_sch_inx-req_qty    = 'X'.
    ls_sch_inx-req_date   = 'X'.
    APPEND ls_sch_inx TO lt_sched_inx.

  ENDLOOP.

  "-----------------------------
  " Logic switch (pricing/condition handling)
  "-----------------------------
  DATA ls_logic TYPE bapisdls.
  CLEAR ls_logic.
  ls_logic-cond_handl = 'X'.

  "-----------------------------
  " Create
  "-----------------------------
  DATA lv_new_vbeln TYPE vbeln_va.
  CLEAR: lv_new_vbeln.

  DATA lt_return TYPE bapiret2_tab.
  CLEAR lt_return.

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

  "-----------------------------
  " Handle return
  "-----------------------------
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
