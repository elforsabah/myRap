METHOD createlanf.

  "=============================================================
  " COPY approach:
  "  1) BAPI_SALESDOCUMENT_COPY  (Contract -> new Sales Order)
  "  2) BAPI_SALESORDER_CHANGE   (set wanted qty + schedule, reject others,
  "                              set partners + plant, header fields)
  "=============================================================

  CONSTANTS:
    gc_auart TYPE auart VALUE 'ZLRA',  "Target Sales Order Type
    gc_rej   TYPE abgru VALUE 'Z0'.   "Rejection reason for not requested items (TVAG)

  "-----------------------------
  " Types
  "-----------------------------
  TYPES: BEGIN OF ty_ctr_pos,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           werks TYPE werks_d,
         END OF ty_ctr_pos.

  TYPES: BEGIN OF ty_want,
           vgpos TYPE posnr_va, "Contract item number
           qty   TYPE kwmeng,
           unit  TYPE vrkme,
         END OF ty_want.

  TYPES: BEGIN OF ty_new_item,
           posnr TYPE posnr_va, "New SO item
           vgpos TYPE posnr_va, "Referenced contract item
         END OF ty_new_item.

  "-----------------------------
  " Data
  "-----------------------------
  DATA: ls_key      LIKE LINE OF keys,
        lv_contract TYPE vbeln_va,
        lv_new_so   TYPE vbeln_va,
        lv_dummy    TYPE vbeln_va,
        lv_matnr    TYPE matnr.

  DATA: lt_return   TYPE bapiret2_tab.

  DATA: lt_ctr_pos  TYPE STANDARD TABLE OF ty_ctr_pos WITH EMPTY KEY,
        ls_ctr_pos  TYPE ty_ctr_pos.

  DATA: lt_want     TYPE HASHED TABLE OF ty_want WITH UNIQUE KEY vgpos.
  FIELD-SYMBOLS <want> TYPE ty_want.

  DATA: lt_new_items TYPE STANDARD TABLE OF ty_new_item WITH EMPTY KEY,
        ls_new_item  TYPE ty_new_item.

  "Partners for change BAPI
  DATA: lt_partners TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY.

  "Change BAPI structures
  DATA: ls_head_in   TYPE bapisdh1,
        ls_head_inx  TYPE bapisdh1x,
        lt_item_in   TYPE bapisditm_tt,
        lt_item_inx  TYPE bapisditmx_tt,
        lt_sch_in    TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
        lt_sch_inx   TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY,
        ls_item_in   TYPE bapisditm,
        ls_item_inx  TYPE bapisditmx,
        ls_sch_in    TYPE bapischdl,
        ls_sch_inx   TYPE bapischdlx.

  "-----------------------------
  " 1) Input
  "-----------------------------
  READ TABLE keys INTO ls_key INDEX 1.
  DATA(ls_input) = ls_key-%param.

  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "-----------------------------
  " 2) Validate contract exists
  "-----------------------------
  SELECT SINGLE vbeln
    FROM vbak
    WHERE vbeln = @lv_contract
    INTO @lv_dummy.

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
  " 3) COPY: contract -> new SO
  "-----------------------------
  CLEAR: lt_return, lv_new_so.

  TRY.
      CALL FUNCTION 'BAPI_SALESDOCUMENT_COPY'
        EXPORTING
          salesdocument    = lv_contract
          documenttype     = gc_auart
          testrun          = space
        IMPORTING
          salesdocument_ex = lv_new_so
        TABLES
          return           = lt_return.
    CATCH cx_sy_message.
      APPEND VALUE #( %msg = new_message(
          id       = sy-msgid
          number   = sy-msgno
          v1       = sy-msgv1
          v2       = sy-msgv2
          v3       = sy-msgv3
          v4       = sy-msgv4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
  ENDTRY.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_e1) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id       = ls_e1-id
        number   = ls_e1-number
        v1       = ls_e1-message_v1
        v2       = ls_e1-message_v2
        v3       = ls_e1-message_v3
        v4       = ls_e1-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  IF lv_new_so IS INITIAL.
    APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Copy did not return a new Sales Order number|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 4) Read contract items (MATNR+POSNR+WERKS)
  "-----------------------------
  SELECT posnr, matnr, werks
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @lt_ctr_pos.

  "-----------------------------
  " 5) Build wanted qty keyed by CONTRACT POSNR
  "-----------------------------
  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    CLEAR ls_ctr_pos.
    READ TABLE lt_ctr_pos INTO ls_ctr_pos WITH KEY matnr = lv_matnr.

    IF sy-subrc <> 0 OR ls_ctr_pos-posnr IS INITIAL.
      APPEND VALUE #( %msg = new_message(
          id       = '00'
          number   = '001'
          v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    ASSIGN lt_want[ vgpos = ls_ctr_pos-posnr ] TO <want>.
    IF sy-subrc = 0.
      <want>-qty  = <want>-qty + ls_pos-menge.
      <want>-unit = ls_pos-meins.
    ELSE.
      INSERT VALUE ty_want(
        vgpos = ls_ctr_pos-posnr
        qty   = ls_pos-menge
        unit  = ls_pos-meins ) INTO TABLE lt_want.
    ENDIF.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 6) Read new order items with reference to contract item (VGPOS)
  "-----------------------------
  SELECT posnr, vgpos
    FROM vbap
    WHERE vbeln = @lv_new_so
    INTO TABLE @lt_new_items.

  "-----------------------------
  " 7) Partners: copy from contract and pass to CHANGE
  "-----------------------------
  SELECT parvw, kunnr
    FROM vbpa
    WHERE vbeln = @lv_contract
      AND kunnr <> ''
      AND parvw IN ('AG','WE','RE','RG','PY')
    INTO TABLE @DATA(lt_vbpa_ctr).

  lt_partners = VALUE #(
    FOR p IN lt_vbpa_ctr
    ( partn_role = p-parvw
      partn_numb = p-kunnr ) ).

  "-----------------------------
  " 8) Header fields (change)
  "-----------------------------
  CLEAR: ls_head_in, ls_head_inx.

  ls_head_in-req_date_h = ls_input-deliverydate.
  ls_head_in-purch_no_c = ls_input-customerref.

  ls_head_inx-updateflag = 'U'.
  ls_head_inx-req_date_h = 'X'.
  ls_head_inx-purch_no_c = 'X'.

  "-----------------------------
  " 9) Build item + schedule changes
  "    - Wanted: qty+unit+plant + schedule qty/date
  "    - Not wanted: reject
  "-----------------------------
  CLEAR: lt_item_in, lt_item_inx, lt_sch_in, lt_sch_inx.

  LOOP AT lt_new_items INTO ls_new_item.

    ASSIGN lt_want[ vgpos = ls_new_item-vgpos ] TO <want>.

    CLEAR: ls_item_in, ls_item_inx, ls_sch_in, ls_sch_inx.

    IF sy-subrc = 0.
      "Wanted item
      ls_item_in-itm_number = ls_new_item-posnr.
      ls_item_in-target_qty = <want>-qty.
      ls_item_in-target_qu  = <want>-unit.

      "Set plant from contract item (very common incompletion requirement)
      READ TABLE lt_ctr_pos INTO ls_ctr_pos WITH KEY posnr = ls_new_item-vgpos.
      IF sy-subrc = 0 AND ls_ctr_pos-werks IS NOT INITIAL.
        ls_item_in-plant = ls_ctr_pos-werks.
      ENDIF.

      APPEND ls_item_in TO lt_item_in.

      ls_item_inx-itm_number = ls_new_item-posnr.
      ls_item_inx-updateflag = 'U'.
      ls_item_inx-target_qty = 'X'.
      ls_item_inx-target_qu  = 'X'.
      IF ls_item_in-plant IS NOT INITIAL.
        ls_item_inx-plant = 'X'.
      ENDIF.
      APPEND ls_item_inx TO lt_item_inx.

      "Schedule line: read actual line from VBEP (no hardcoding)
      SELECT SINGLE etenr
        FROM vbep
        WHERE vbeln = @lv_new_so
          AND posnr = @ls_new_item-posnr
        INTO @DATA(lv_etenr).

      IF lv_etenr IS INITIAL.
        lv_etenr = '0001'.
      ENDIF.

      ls_sch_in-itm_number = ls_new_item-posnr.
      ls_sch_in-sched_line = lv_etenr.
      ls_sch_in-req_qty    = <want>-qty.
      ls_sch_in-req_date   = ls_input-deliverydate.
      APPEND ls_sch_in TO lt_sch_in.

      ls_sch_inx-itm_number = ls_new_item-posnr.
      ls_sch_inx-sched_line = lv_etenr.
      ls_sch_inx-updateflag = 'U'.
      ls_sch_inx-req_qty    = 'X'.
      ls_sch_inx-req_date   = 'X'.
      APPEND ls_sch_inx TO lt_sch_inx.

    ELSE.
      "Not wanted -> reject
      ls_item_in-itm_number = ls_new_item-posnr.
      ls_item_in-reason_rej = gc_rej.
      APPEND ls_item_in TO lt_item_in.

      ls_item_inx-itm_number = ls_new_item-posnr.
      ls_item_inx-updateflag = 'U'.
      ls_item_inx-reason_rej = 'X'.
      APPEND ls_item_inx TO lt_item_inx.
    ENDIF.

  ENDLOOP.

  "-----------------------------
  " 10) CHANGE the copied order
  "-----------------------------
  CLEAR lt_return.

  TRY.
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
          schedule_linesx  = lt_sch_inx
          order_partners   = lt_partners.
    CATCH cx_sy_message.
      APPEND VALUE #( %msg = new_message(
          id       = sy-msgid
          number   = sy-msgno
          v1       = sy-msgv1
          v2       = sy-msgv2
          v3       = sy-msgv3
          v4       = sy-msgv4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
  ENDTRY.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_e2) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id       = ls_e2-id
        number   = ls_e2-number
        v1       = ls_e2-message_v1
        v2       = ls_e2-message_v2
        v3       = ls_e2-message_v3
        v4       = ls_e2-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "No explicit COMMIT here (RAP LUW)

  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_so
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (COPY)' ) )
    ) )
  ).

ENDMETHOD.
