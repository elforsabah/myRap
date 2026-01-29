METHOD createlanf.

  "=============================================================
  " OPTION B: BAPI_SALESDOCUMENT_COPY + BAPI_SALESORDER_CHANGE
  "=============================================================

  CONSTANTS gc_rej TYPE abgru VALUE 'Z0'.  "<<< change to your valid rejection reason

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA(lv_contract) TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "---- Validate contract exists
  SELECT SINGLE vbeln
    FROM vbak
    WHERE vbeln = @lv_contract
    INTO @DATA(lv_dummy).

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id='00' number='001'
      v1 = |Contract { lv_contract } not found|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "---- Copy contract to new sales order
  DATA: lt_return TYPE bapiret2_tab,
        lv_new_so TYPE vbeln_va.

  CALL FUNCTION 'BAPI_SALESDOCUMENT_COPY'
    EXPORTING
      salesdocument    = lv_contract
      documenttype     = 'ZLRA'
      testrun          = space
    IMPORTING
      salesdocument_ex = lv_new_so
    TABLES
      return           = lt_return.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_e1) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id = ls_e1-id number = ls_e1-number
        v1 = ls_e1-message_v1 v2 = ls_e1-message_v2 v3 = ls_e1-message_v3 v4 = ls_e1-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  IF lv_new_so IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id='00' number='001'
      v1 = |Copy did not return a new sales order number|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "---- Contract positions: posnr/matnr
  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_ctr_pos).

  IF lt_ctr_pos IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id='00' number='001'
      v1 = |No VBAP items for contract { lv_contract }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "---- Build wanted quantities keyed by contract POSNR
  TYPES: BEGIN OF ty_want,
           posnr TYPE posnr_va,
           qty   TYPE kwmeng,
           unit  TYPE vrkme,
         END OF ty_want.
  DATA lt_want TYPE HASHED TABLE OF ty_want WITH UNIQUE KEY posnr.

  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    DATA(lv_matnr) TYPE matnr.
    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_ctr_pos INTO DATA(ls_ctr) WITH KEY matnr = lv_matnr.
    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message(
        id='00' number='001'
        v1 = |Material { ls_pos-matnr } not found in contract { lv_contract }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    INSERT VALUE ty_want(
      posnr = ls_ctr-posnr
      qty   = ls_pos-menge
      unit  = ls_pos-meins ) INTO TABLE lt_want.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "---- Prepare CHANGE structures (use contract posnr; copy normally keeps same numbering)
  DATA: ls_head_in  TYPE bapisdhd1,
        ls_head_inx TYPE bapisdhd1x,
        lt_item_in  TYPE bapisditm_tt,
        lt_item_inx TYPE bapisditmx_tt,
        lt_sch_in   TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
        lt_sch_inx  TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY.

  ls_head_in-req_date_h  = ls_input-deliverydate.
  ls_head_in-purch_no_c  = ls_input-customerref.

  ls_head_inx-updateflag = 'U'.
  ls_head_inx-req_date_h = 'X'.
  ls_head_inx-purch_no_c = 'X'.

  LOOP AT lt_ctr_pos INTO DATA(ls_ctr2).

    READ TABLE lt_want INTO DATA(ls_w) WITH KEY posnr = ls_ctr2-posnr.

    IF sy-subrc = 0.
      "Wanted item: set qty + unit and schedule line 0001
      APPEND VALUE bapisditm(
        itm_number = ls_ctr2-posnr
        target_qty = ls_w-qty
        target_qu  = ls_w-unit ) TO lt_item_in.

      APPEND VALUE bapisditmx(
        itm_number = ls_ctr2-posnr
        updateflag = 'U'
        target_qty = 'X'
        target_qu  = 'X' ) TO lt_item_inx.

      APPEND VALUE bapischdl(
        itm_number = ls_ctr2-posnr
        sched_line = '0001'
        req_qty    = ls_w-qty
        req_date   = ls_input-deliverydate ) TO lt_sch_in.

      APPEND VALUE bapischdlx(
        itm_number = ls_ctr2-posnr
        sched_line = '0001'
        updateflag = 'U'
        req_qty    = 'X'
        req_date   = 'X' ) TO lt_sch_inx.

    ELSE.
      "Not wanted: reject item
      APPEND VALUE bapisditm(
        itm_number = ls_ctr2-posnr
        reason_rej = gc_rej ) TO lt_item_in.

      APPEND VALUE bapisditmx(
        itm_number = ls_ctr2-posnr
        updateflag = 'U'
        reason_rej = 'X' ) TO lt_item_inx.
    ENDIF.

  ENDLOOP.

  CLEAR lt_return.

  CALL FUNCTION 'BAPI_SALESORDER_CHANGE'
    EXPORTING
      salesdocument    = lv_new_so
      order_header_in  = ls_head_in
      order_header_inx = ls_head_inx
    TABLES
      return           = lt_return
      order_item_in    = lt_item_in
      order_item_inx   = lt_item_inx
      schedule_lines   = lt_sch_in
      schedule_linesx  = lt_sch_inx.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_e2) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id = ls_e2-id number = ls_e2-number
        v1 = ls_e2-message_v1 v2 = ls_e2-message_v2 v3 = ls_e2-message_v3 v4 = ls_e2-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "No BAPI_TRANSACTION_COMMIT here; RAP will COMMIT the LUW afterwards

  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_so
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (COPY)' ) )
    ) )
  ).

ENDMETHOD.
