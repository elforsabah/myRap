CLASS lhc_lanfroot DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    TYPES tt_lanf_position_input TYPE STANDARD TABLE OF zi_lanf_position_input WITH DEFAULT KEY.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR lanfroot RESULT result.

    METHODS read FOR READ
      IMPORTING keys FOR READ lanfroot RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK lanfroot.

    METHODS attachdocument FOR MODIFY
      IMPORTING keys FOR ACTION  lanfroot~attachdocument RESULT result.

    METHODS createlanf FOR MODIFY
      IMPORTING keys FOR ACTION lanfroot~createlanf RESULT result.

    METHODS map_positions_to_contract
      IMPORTING iv_contract_vbeln TYPE vbeln_va
                it_positions      TYPE tt_lanf_position_input
      CHANGING  ct_order_items    TYPE  bapisditm_tt
                ct_order_itemsx   TYPE  bapisditmx_tt
                ct_return         TYPE bapiret2_tab.


ENDCLASS.

CLASS lhc_lanfroot IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.


*METHOD createlanf.
*
*  "=============================================================
*  " CREATEFROMDAT2 approach (recommended)
*  " Fixes M_/011 by aggregating input positions so each
*  " contract item (POSNR) appears only once in BAPI tables.
*  "=============================================================
*
*  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "Target sales order type
*
*  "-----------------------------
*  " Input
*  "-----------------------------
*  DATA ls_key LIKE LINE OF keys.
*  READ TABLE keys INTO ls_key INDEX 1.
*  DATA(ls_input) = ls_key-%param.
*
*  DATA lv_contract TYPE vbeln_va.
*  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.
*
*  "-----------------------------
*  " Validate contract + get org data
*  "-----------------------------
*  DATA ls_contract_hdr TYPE vbak.
*  SELECT SINGLE *
*    FROM vbak
*    WHERE vbeln = @lv_contract
*    INTO @ls_contract_hdr.
*
*  IF sy-subrc <> 0.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |Contract { lv_contract } not found|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " Read contract items (for mapping MATNR -> POSNR and plant)
*  "-----------------------------
*  TYPES: BEGIN OF ty_ctr_item,
*           posnr TYPE posnr_va,
*           matnr TYPE matnr,
*           werks TYPE werks_d,
*         END OF ty_ctr_item.
*
*  DATA lt_ctr_items TYPE STANDARD TABLE OF ty_ctr_item WITH EMPTY KEY.
*
*  SELECT posnr, matnr, werks
*    FROM vbap
*    WHERE vbeln = @lv_contract
*    INTO TABLE @lt_ctr_items.
*
*  IF lt_ctr_items IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |No items found in contract { lv_contract }|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " Aggregate input positions by contract POSNR (fix M_/011)
*  "-----------------------------
*  TYPES: BEGIN OF ty_agg,
*           posnr TYPE posnr_va,   "contract item posnr
*           matnr TYPE matnr,      "normalized matnr
*           werks TYPE werks_d,
*           meins TYPE vrkme,
*           qty   TYPE kwmeng,
*         END OF ty_agg.
*
*  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
*  FIELD-SYMBOLS <agg> TYPE ty_agg.
*
*  DATA lv_matnr TYPE matnr.
*
*  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.
*
*    lv_matnr = ls_pos-matnr.
*    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
*      EXPORTING input  = lv_matnr
*      IMPORTING output = lv_matnr.
*
*    READ TABLE lt_ctr_items INTO DATA(ls_ctr) WITH KEY matnr = lv_matnr.
*    IF sy-subrc <> 0 OR ls_ctr-posnr IS INITIAL.
*      APPEND VALUE #( %msg = new_message(
*        id       = '00'
*        number   = '001'
*        v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      CONTINUE.
*    ENDIF.
*
*    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
*    IF sy-subrc = 0.
*      "same contract item appears again -> sum qty (prevents duplicate ITM_NUMBER)
*      IF <agg>-meins <> ls_pos-meins.
*        APPEND VALUE #( %msg = new_message(
*          id       = '00'
*          number   = '001'
*          v1       = |Same item { ls_ctr-posnr } has different UoM ({ <agg>-meins } vs { ls_pos-meins })|
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*        CONTINUE.
*      ENDIF.
*      <agg>-qty = <agg>-qty + ls_pos-menge.
*    ELSE.
*      INSERT VALUE ty_agg(
*        posnr = ls_ctr-posnr
*        matnr = lv_matnr
*        werks = ls_ctr-werks
*        meins = ls_pos-meins
*        qty   = ls_pos-menge ) INTO TABLE lt_agg.
*    ENDIF.
*
*  ENDLOOP.
*
*  IF reported-lanfroot IS NOT INITIAL.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  IF lt_agg IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |No valid items found (all quantities <= 0 or mapping failed)|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " Partners from contract
*  "-----------------------------
*  DATA lt_partners TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY.
*
*  SELECT parvw, kunnr
*    FROM vbpa
*    WHERE vbeln = @lv_contract
*      AND kunnr <> ''
*      AND parvw IN ('AG','WE','RE','RG','PY')
*    INTO TABLE @DATA(lt_vbpa).
*
*  lt_partners = VALUE #(
*    FOR p IN lt_vbpa
*    ( partn_role = p-parvw
*      partn_numb = p-kunnr ) ).
*
*  "-----------------------------
*  " Header (DAT2)
*  "-----------------------------
*  DATA: ls_head_in  TYPE bapisdhd1,
*        ls_head_inx TYPE bapisdhd1x.
*
*  CLEAR: ls_head_in, ls_head_inx.
*
*  ls_head_in-doc_type    = gc_auart.
*  ls_head_in-sales_org   = ls_contract_hdr-vkorg.
*  ls_head_in-distr_chan  = ls_contract_hdr-vtweg.
*  ls_head_in-division    = ls_contract_hdr-spart.
*
*  "reference to contract
*  ls_head_in-ref_doc     = lv_contract.
*  ls_head_in-refdoc_cat  = 'G'.
*
*  ls_head_in-req_date_h  = ls_input-deliverydate.
*  ls_head_in-purch_no_c  = ls_input-customerref.
*
*  ls_head_inx-updateflag = 'I'.
*  ls_head_inx-doc_type   = 'X'.
*  ls_head_inx-sales_org  = 'X'.
*  ls_head_inx-distr_chan = 'X'.
*  ls_head_inx-division   = 'X'.
*  ls_head_inx-ref_doc    = 'X'.
*  ls_head_inx-refdoc_cat = 'X'.
*  ls_head_inx-req_date_h = 'X'.
*  ls_head_inx-purch_no_c = 'X'.
*
*  "-----------------------------
*  " Items + Schedules (ONE per POSNR)
*  "-----------------------------
*  DATA: lt_items_in  TYPE bapisditm_tt,
*        lt_items_inx TYPE bapisditmx_tt,
*        lt_sched_in  TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
*        lt_sched_inx TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY.
*
*  DATA: ls_item_in  TYPE bapisditm,
*        ls_item_inx TYPE bapisditmx,
*        ls_sch_in   TYPE bapischdl,
*        ls_sch_inx  TYPE bapischdlx.
*
*  CLEAR: lt_items_in, lt_items_inx, lt_sched_in, lt_sched_inx.
*
*  LOOP AT lt_agg INTO DATA(ls_agg).
*
*    CLEAR: ls_item_in, ls_item_inx, ls_sch_in, ls_sch_inx.
*
*    "Item
*    ls_item_in-itm_number = ls_agg-posnr.
*    ls_item_in-material   = ls_agg-matnr.
*    ls_item_in-target_qty = ls_agg-qty.
*    ls_item_in-target_qu  = ls_agg-meins.
*
*    ls_item_in-ref_doc    = lv_contract.
*    ls_item_in-ref_doc_it = ls_agg-posnr.
*    ls_item_in-ref_doc_ca = 'G'.
*
*    IF ls_agg-werks IS NOT INITIAL.
*      ls_item_in-plant = ls_agg-werks.
*    ENDIF.
*
*    APPEND ls_item_in TO lt_items_in.
*
*    "Item X
*    ls_item_inx-itm_number = ls_agg-posnr.
*    ls_item_inx-updateflag = 'I'.
*    ls_item_inx-material   = 'X'.
*    ls_item_inx-target_qty = 'X'.
*    ls_item_inx-target_qu  = 'X'.
*    ls_item_inx-ref_doc    = 'X'.
*    ls_item_inx-ref_doc_it = 'X'.
*    ls_item_inx-ref_doc_ca = 'X'.
*    IF ls_item_in-plant IS NOT INITIAL.
*      ls_item_inx-plant = 'X'.
*    ENDIF.
*
*    APPEND ls_item_inx TO lt_items_inx.
*
*    "Schedule line (helps completeness & consistency)
*    ls_sch_in-itm_number = ls_agg-posnr.
*    ls_sch_in-sched_line = '0001'.
*    ls_sch_in-req_qty    = ls_agg-qty.
*    ls_sch_in-req_date   = ls_input-deliverydate.
*    APPEND ls_sch_in TO lt_sched_in.
*
*    ls_sch_inx-itm_number = ls_agg-posnr.
*    ls_sch_inx-sched_line = '0001'.
*    ls_sch_inx-updateflag = 'I'.
*    ls_sch_inx-req_qty    = 'X'.
*    ls_sch_inx-req_date   = 'X'.
*    APPEND ls_sch_inx TO lt_sched_inx.
*
*  ENDLOOP.
*
*  "-----------------------------
*  " Logic switch (pricing/condition handling)
*  "-----------------------------
*  DATA ls_logic TYPE bapisdls.
*  CLEAR ls_logic.
*  ls_logic-cond_handl = 'X'.
*
*  "-----------------------------
*  " Create
*  "-----------------------------
*  DATA lv_new_vbeln TYPE vbeln_va.
*  CLEAR: lv_new_vbeln.
*
*  DATA lt_return TYPE bapiret2_tab.
*  CLEAR lt_return.
*
*  CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
*    EXPORTING
*      order_header_in      = ls_head_in
*      order_header_inx     = ls_head_inx
*      logic_switch         = ls_logic
*    IMPORTING
*      salesdocument        = lv_new_vbeln
*    TABLES
*      return               = lt_return
*      order_items_in       = lt_items_in
*      order_items_inx      = lt_items_inx
*      order_schedules_in   = lt_sched_in
*      order_schedules_inx  = lt_sched_inx
*      order_partners       = lt_partners.
*
*  "-----------------------------
*  " Handle return
*  "-----------------------------
*  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
*    LOOP AT lt_return INTO DATA(ls_err) WHERE type CA 'EA'.
*      APPEND VALUE #( %msg = new_message(
*        id       = ls_err-id
*        number   = ls_err-number
*        v1       = ls_err-message_v1
*        v2       = ls_err-message_v2
*        v3       = ls_err-message_v3
*        v4       = ls_err-message_v4
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    ENDLOOP.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  IF lv_new_vbeln IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |BAPI returned no document number|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "Success
*  result = VALUE #(
*    ( %param = VALUE #(
*        techkey   = 'X'
*        vbeln     = lv_new_vbeln
*        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (CREATEFROMDAT2)' ) )
*    ) )
*  ).
*
*ENDMETHOD.


*METHOD createlanf.
*
*  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "your target doc type
*
*  "------------------------------------------------------------
*  " Input
*  "------------------------------------------------------------
*  DATA ls_key LIKE LINE OF keys.
*  READ TABLE keys INTO ls_key INDEX 1.
*  DATA(ls_input) = ls_key-%param.
*
*  DATA lv_refdoc TYPE vbeln_va.
*  lv_refdoc = |{ ls_input-contractvbeln ALPHA = IN }|.
*
*  "------------------------------------------------------------
*  " Validate reference doc exists + org data
*  "------------------------------------------------------------
*  DATA ls_ref_hdr TYPE vbak.
*  SELECT SINGLE *
*    FROM vbak
*    WHERE vbeln = @lv_refdoc
*    INTO @ls_ref_hdr.
*
*  IF sy-subrc <> 0.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |Reference document { lv_refdoc } not found|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "------------------------------------------------------------
*  " Determine document category (TVAK-VBTYP) for the target AUART
*  "------------------------------------------------------------
*  DATA lv_vbtyp TYPE tvak-vbtyp.
*  SELECT SINGLE vbtyp
*    FROM tvak
*    WHERE auart = @gc_auart
*    INTO @lv_vbtyp.
*
*  IF sy-subrc <> 0 OR lv_vbtyp IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |No TVAK entry / VBTYP found for AUART { gc_auart }|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "------------------------------------------------------------
*  " Read reference items for MATNR->POSNR mapping + plant
*  "------------------------------------------------------------
*  TYPES: BEGIN OF ty_ref_item,
*           posnr TYPE posnr_va,
*           matnr TYPE matnr,
*           werks TYPE werks_d,
*         END OF ty_ref_item.
*  DATA lt_ref_items TYPE STANDARD TABLE OF ty_ref_item WITH EMPTY KEY.
*
*  SELECT posnr, matnr, werks
*    FROM vbap
*    WHERE vbeln = @lv_refdoc
*    INTO TABLE @lt_ref_items.
*
*  "------------------------------------------------------------
*  " Aggregate positions by reference POSNR (prevents duplicate ITM_NUMBER => M_/011)
*  "------------------------------------------------------------
*  TYPES: BEGIN OF ty_agg,
*           posnr TYPE posnr_va,
*           matnr TYPE matnr,
*           werks TYPE werks_d,
*           meins TYPE vrkme,
*           qty   TYPE kwmeng,
*         END OF ty_agg.
*  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
*  FIELD-SYMBOLS <agg> TYPE ty_agg.
*
*  DATA lv_matnr TYPE matnr.
*
*  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.
*
*    lv_matnr = ls_pos-matnr.
*    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
*      EXPORTING input  = lv_matnr
*      IMPORTING output = lv_matnr.
*
*    READ TABLE lt_ref_items INTO DATA(ls_ref_it) WITH KEY matnr = lv_matnr.
*    IF sy-subrc <> 0 OR ls_ref_it-posnr IS INITIAL.
*      APPEND VALUE #( %msg = new_message(
*        id       = '00'
*        number   = '001'
*        v1       = |Material { ls_pos-matnr } not found in reference { lv_refdoc }|
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      CONTINUE.
*    ENDIF.
*
*    ASSIGN lt_agg[ posnr = ls_ref_it-posnr ] TO <agg>.
*    IF sy-subrc = 0.
*      IF <agg>-meins <> ls_pos-meins.
*        APPEND VALUE #( %msg = new_message(
*          id       = '00'
*          number   = '001'
*          v1       = |Same item { ls_ref_it-posnr } has different UoM ({ <agg>-meins } vs { ls_pos-meins })|
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*        CONTINUE.
*      ENDIF.
*      <agg>-qty = <agg>-qty + ls_pos-menge.
*    ELSE.
*      INSERT VALUE ty_agg(
*        posnr = ls_ref_it-posnr
*        matnr = lv_matnr
*        werks = ls_ref_it-werks
*        meins = ls_pos-meins
*        qty   = ls_pos-menge ) INTO TABLE lt_agg.
*    ENDIF.
*
*  ENDLOOP.
*
*  IF reported-lanfroot IS NOT INITIAL OR lt_agg IS INITIAL.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "------------------------------------------------------------
*  " Partners from reference doc
*  "------------------------------------------------------------
*  DATA lt_partners_dat2 TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY.
*  DATA lt_partners_gen  TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.
*
*  SELECT parvw, kunnr
*    FROM vbpa
*    WHERE vbeln = @lv_refdoc
*      AND kunnr <> ''
*      AND parvw IN ('AG','WE','RE','RG','PY')
*    INTO TABLE @DATA(lt_vbpa).
*
*  lt_partners_dat2 = VALUE #(
*    FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
*
*  lt_partners_gen = VALUE #(
*    FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
*
*  "============================================================
*  " BRANCH A: VBTYP = 'C' => Sales Order API (BUS2032)
*  "============================================================
*  IF lv_vbtyp = 'C'.
*
*    DATA: ls_head_in   TYPE bapisdhd1,
*          ls_head_inx  TYPE bapisdhd1x,
*          ls_logic     TYPE bapisdls,
*          lt_items_in  TYPE bapisditm_tt,
*          lt_items_inx TYPE bapisditmx_tt,
*          lt_sch_in    TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
*          lt_sch_inx   TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY,
*          lt_return    TYPE bapiret2_tab,
*          lv_new_vbeln TYPE vbeln_va.
*
*    CLEAR: ls_head_in, ls_head_inx, ls_logic, lt_items_in, lt_items_inx, lt_sch_in, lt_sch_inx, lt_return, lv_new_vbeln.
*
*    ls_head_in-doc_type    = gc_auart.
*    ls_head_in-sales_org   = ls_ref_hdr-vkorg.
*    ls_head_in-distr_chan  = ls_ref_hdr-vtweg.
*    ls_head_in-division    = ls_ref_hdr-spart.
*    ls_head_in-ref_doc     = lv_refdoc.
*    ls_head_in-refdoc_cat  = 'G'. "contract reference in your scenario
*    ls_head_in-req_date_h  = ls_input-deliverydate.
*    ls_head_in-purch_no_c  = ls_input-customerref.
*
*    ls_head_inx-updateflag = 'I'.
*    ls_head_inx-doc_type   = 'X'.
*    ls_head_inx-sales_org  = 'X'.
*    ls_head_inx-distr_chan = 'X'.
*    ls_head_inx-division   = 'X'.
*    ls_head_inx-ref_doc    = 'X'.
*    ls_head_inx-refdoc_cat = 'X'.
*    ls_head_inx-req_date_h = 'X'.
*    ls_head_inx-purch_no_c = 'X'.
*
*    ls_logic-cond_handl = 'X'.
*
*    LOOP AT lt_agg INTO DATA(ls_agg).
*
*      DATA: ls_item_in  TYPE bapisditm,
*            ls_item_inx TYPE bapisditmx,
*            ls_sch1     TYPE bapischdl,
*            ls_sch1x    TYPE bapischdlx.
*
*      CLEAR: ls_item_in, ls_item_inx, ls_sch1, ls_sch1x.
*
*      ls_item_in-itm_number = ls_agg-posnr.
*      ls_item_in-material   = ls_agg-matnr.
*      ls_item_in-target_qty = ls_agg-qty.
*      ls_item_in-target_qu  = ls_agg-meins.
*      ls_item_in-ref_doc    = lv_refdoc.
*      ls_item_in-ref_doc_it = ls_agg-posnr.
*      ls_item_in-ref_doc_ca = 'G'.
*      IF ls_agg-werks IS NOT INITIAL.
*        ls_item_in-plant = ls_agg-werks.
*      ENDIF.
*      APPEND ls_item_in TO lt_items_in.
*
*      ls_item_inx-itm_number = ls_agg-posnr.
*      ls_item_inx-updateflag = 'I'.
*      ls_item_inx-material   = 'X'.
*      ls_item_inx-target_qty = 'X'.
*      ls_item_inx-target_qu  = 'X'.
*      ls_item_inx-ref_doc    = 'X'.
*      ls_item_inx-ref_doc_it = 'X'.
*      ls_item_inx-ref_doc_ca = 'X'.
*      IF ls_item_in-plant IS NOT INITIAL.
*        ls_item_inx-plant = 'X'.
*      ENDIF.
*      APPEND ls_item_inx TO lt_items_inx.
*
*      ls_sch1-itm_number = ls_agg-posnr.
*      ls_sch1-sched_line = '0001'.
*      ls_sch1-req_qty    = ls_agg-qty.
*      ls_sch1-req_date   = ls_input-deliverydate.
*      APPEND ls_sch1 TO lt_sch_in.
*
*      ls_sch1x-itm_number = ls_agg-posnr.
*      ls_sch1x-sched_line = '0001'.
*      ls_sch1x-updateflag = 'I'.
*      ls_sch1x-req_qty    = 'X'.
*      ls_sch1x-req_date   = 'X'.
*      APPEND ls_sch1x TO lt_sch_inx.
*
*    ENDLOOP.
*
*    TRY.
*        CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
*          EXPORTING
*            order_header_in      = ls_head_in
*            order_header_inx     = ls_head_inx
*            logic_switch         = ls_logic
*          IMPORTING
*            salesdocument        = lv_new_vbeln
*          TABLES
*            return               = lt_return
*            order_items_in       = lt_items_in
*            order_items_inx      = lt_items_inx
*            order_schedules_in   = lt_sch_in
*            order_schedules_inx  = lt_sch_inx
*            order_partners       = lt_partners_dat2.
*      CATCH cx_aab_activation.
*        APPEND VALUE #( %msg = new_message(
*          id       = sy-msgid
*          number   = sy-msgno
*          v1       = sy-msgv1
*          v2       = sy-msgv2
*          v3       = sy-msgv3
*          v4       = sy-msgv4
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*        failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*        RETURN.
*    ENDTRY.
*
*    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ) OR lv_new_vbeln IS INITIAL.
*      LOOP AT lt_return INTO DATA(ls_e1) WHERE type CA 'EA'.
*        APPEND VALUE #( %msg = new_message(
*          id       = ls_e1-id
*          number   = ls_e1-number
*          v1       = ls_e1-message_v1
*          v2       = ls_e1-message_v2
*          v3       = ls_e1-message_v3
*          v4       = ls_e1-message_v4
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      ENDLOOP.
*      IF lv_new_vbeln IS INITIAL.
*        APPEND VALUE #( %msg = new_message(
*          id       = '00'
*          number   = '001'
*          v1       = |No document created|
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      ENDIF.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*    ENDIF.
*
*    result = VALUE #(
*      ( %param = VALUE #(
*          techkey   = 'X'
*          vbeln     = lv_new_vbeln
*          _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (DAT2)' ) )
*      ) )
*    ).
*
*    RETURN.
*  ENDIF.
*
*  "============================================================
*  " BRANCH B: VBTYP <> 'C' (e.g. 'L') => Generic Sales Doc BAPI
*  "============================================================
*  DATA lv_busobj TYPE swo_objtyp.
*  CLEAR lv_busobj.
*
*  CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
*    EXPORTING
*      i_document_type   = lv_vbtyp
*    IMPORTING
*      e_business_object = lv_busobj.
*
*  IF lv_busobj IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |Cannot determine BOR object for doc category { lv_vbtyp }|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  DATA: ls_head_gen TYPE bapisdhead,
*        lt_item_gen TYPE STANDARD TABLE OF bapiitemin WITH DEFAULT KEY,
*        lt_ret2     TYPE bapiret2_tab,
*        ls_ret1     TYPE bapireturn1,
*        lv_new_doc  TYPE vbeln_va.
*
*  CLEAR: ls_head_gen, lt_item_gen, lt_ret2, ls_ret1, lv_new_doc.
*
*  ls_head_gen-doc_type   = gc_auart.
*  ls_head_gen-sales_org  = ls_ref_hdr-vkorg.
*  ls_head_gen-distr_chan = ls_ref_hdr-vtweg.
*  ls_head_gen-division   = ls_ref_hdr-spart.
*
*  ls_head_gen-ref_doc    = lv_refdoc.
*  ls_head_gen-ref_doc_ca = 'G'.  "your reference is contract; adjust if not a contract
*  ls_head_gen-req_date_h = ls_input-deliverydate.
*  ls_head_gen-purch_no_c = ls_input-customerref.
*
*  LOOP AT lt_agg INTO DATA(ls_agg2).
*    APPEND VALUE bapiitemin(
*      itm_number = ls_agg2-posnr
*      material   = ls_agg2-matnr
*      target_qty = ls_agg2-qty
*      target_qu  = ls_agg2-meins
*      ref_doc    = lv_refdoc
*      ref_doc_it = ls_agg2-posnr
*      ref_doc_ca = 'G'
*      plant      = ls_agg2-werks ) TO lt_item_gen.
*  ENDLOOP.
*
*  TRY.
*      CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
*        EXPORTING
*          order_header_in = ls_head_gen
*          business_object = lv_busobj
*          without_commit  = abap_true
*        IMPORTING
*          salesdocument   = lv_new_doc
*          return          = ls_ret1
*        TABLES
*          order_items_in  = lt_item_gen
*          order_partners  = lt_partners_gen.
*    CATCH cx_sy_assign_error.
*      APPEND VALUE #( %msg = new_message(
*        id       = sy-msgid
*        number   = sy-msgno
*        v1       = sy-msgv1
*        v2       = sy-msgv2
*        v3       = sy-msgv3
*        v4       = sy-msgv4
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*  ENDTRY.
*
*  "Convert RETURN1 to RAP message
*  IF ls_ret1-type CA 'EAX' OR lv_new_doc IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = ls_ret1-id
*      number   = ls_ret1-number
*      v1       = ls_ret1-message_v1
*      v2       = ls_ret1-message_v2
*      v3       = ls_ret1-message_v3
*      v4       = ls_ret1-message_v4
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  result = VALUE #(
*    ( %param = VALUE #(
*        techkey   = 'X'
*        vbeln     = lv_new_doc
*        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (GENERIC)' ) )
*    ) )
*  ).
*
*ENDMETHOD.



*METHOD createlanf.
*
*  "=============================================================
*  " Robust create with reference + aggregation + German language
*  " - Fixes M_/011 by aggregating positions (no duplicate ITM_NUMBER)
*  " - Fixes V1/761 by choosing correct BAPI depending on VBTYP
*  " - Fixes V2/157 (missing division text EN) by forcing language = 'D'
*  "   during BAPI call (your TSPAT is German-only)
*  "=============================================================
*
*  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "Target doc type
*
*  "-----------------------------
*  " 1) Input
*  "-----------------------------
*  DATA ls_key LIKE LINE OF keys.
*  READ TABLE keys INTO ls_key INDEX 1.
*  DATA(ls_input) = ls_key-%param.
*
*  DATA lv_refdoc TYPE vbeln_va.
*  lv_refdoc = |{ ls_input-contractvbeln ALPHA = IN }|.
*
*  "-----------------------------
*  " 2) Validate reference document exists + org data
*  "-----------------------------
*  DATA ls_ref_hdr TYPE vbak.
*  SELECT SINGLE *
*    FROM vbak
*    WHERE vbeln = @lv_refdoc
*    INTO @ls_ref_hdr.
*
*  IF sy-subrc <> 0.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |Reference document { lv_refdoc } not found|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " 3) Determine sales doc category (VBTYP) for target AUART
*  "-----------------------------
*  DATA lv_vbtyp TYPE tvak-vbtyp.
*  SELECT SINGLE vbtyp
*    FROM tvak
*    WHERE auart = @gc_auart
*    INTO @lv_vbtyp.
*
*  IF sy-subrc <> 0 OR lv_vbtyp IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |No VBTYP found for AUART { gc_auart } (TVAK)|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " 4) Read reference items for MATNR->POSNR mapping + plant
*  "-----------------------------
*  TYPES: BEGIN OF ty_ref_item,
*           posnr TYPE posnr_va,
*           matnr TYPE matnr,
*           werks TYPE werks_d,
*         END OF ty_ref_item.
*  DATA lt_ref_items TYPE STANDARD TABLE OF ty_ref_item WITH EMPTY KEY.
*
*  SELECT posnr, matnr, werks
*    FROM vbap
*    WHERE vbeln = @lv_refdoc
*    INTO TABLE @lt_ref_items.
*
*  IF lt_ref_items IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00'
*      number   = '001'
*      v1       = |No items found in reference { lv_refdoc }|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " 5) Aggregate input positions by reference POSNR (fix M_/011)
*  "-----------------------------
*  TYPES: BEGIN OF ty_agg,
*           posnr TYPE posnr_va,
*           matnr TYPE matnr,
*           werks TYPE werks_d,
*           meins TYPE vrkme,
*           qty   TYPE kwmeng,
*         END OF ty_agg.
*  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
*  FIELD-SYMBOLS <agg> TYPE ty_agg.
*
*  DATA lv_matnr TYPE matnr.
*
*  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.
*
*    lv_matnr = ls_pos-matnr.
*    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
*      EXPORTING input  = lv_matnr
*      IMPORTING output = lv_matnr.
*
*    READ TABLE lt_ref_items INTO DATA(ls_ref_it) WITH KEY matnr = lv_matnr.
*    IF sy-subrc <> 0 OR ls_ref_it-posnr IS INITIAL.
*      APPEND VALUE #( %msg = new_message(
*        id       = '00'
*        number   = '001'
*        v1       = |Material { ls_pos-matnr } not found in reference { lv_refdoc }|
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      CONTINUE.
*    ENDIF.
*
*    ASSIGN lt_agg[ posnr = ls_ref_it-posnr ] TO <agg>.
*    IF sy-subrc = 0.
*      "same ref item occurs again -> sum qty (prevents duplicate ITM_NUMBER)
*      IF <agg>-meins <> ls_pos-meins.
*        APPEND VALUE #( %msg = new_message(
*          id       = '00'
*          number   = '001'
*          v1       = |Same item { ls_ref_it-posnr } has different UoM ({ <agg>-meins } vs { ls_pos-meins })|
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*        CONTINUE.
*      ENDIF.
*      <agg>-qty = <agg>-qty + ls_pos-menge.
*    ELSE.
*      INSERT VALUE ty_agg(
*        posnr = ls_ref_it-posnr
*        matnr = lv_matnr
*        werks = ls_ref_it-werks
*        meins = ls_pos-meins
*        qty   = ls_pos-menge ) INTO TABLE lt_agg.
*    ENDIF.
*
*  ENDLOOP.
*
*  IF reported-lanfroot IS NOT INITIAL OR lt_agg IS INITIAL.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "-----------------------------
*  " 6) Partners from reference
*  "-----------------------------
*  DATA lt_partners_dat2 TYPE STANDARD TABLE OF bapiparnr  WITH DEFAULT KEY.
*  DATA lt_partners_gen  TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.
*
*  SELECT parvw, kunnr
*    FROM vbpa
*    WHERE vbeln = @lv_refdoc
*      AND kunnr <> ''
*      AND parvw IN ('AG','WE','RE','RG','PY')
*    INTO TABLE @DATA(lt_vbpa).
*
*  lt_partners_dat2 = VALUE #( FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
*  lt_partners_gen  = VALUE #( FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
*
*  "=============================================================
*  " 7) Force German during BAPI to avoid "Missing text ... language EN"
*  "=============================================================
*  DATA(lv_old_langu) = sy-langu.
*  DATA lv_bapi_langu TYPE sylangu VALUE 'D'.  "German
*
*  TRY.
*      SET LOCALE LANGUAGE lv_bapi_langu.
*
*      "=========================================================
*      " Branch A: VBTYP = 'C' -> use BAPI_SALESORDER_CREATEFROMDAT2
*      "=========================================================
*      IF lv_vbtyp = 'C'.
*
*        DATA: ls_head_in   TYPE bapisdhd1,
*              ls_head_inx  TYPE bapisdhd1x,
*              ls_logic     TYPE bapisdls,
*              lt_items_in  TYPE bapisditm_tt,
*              lt_items_inx TYPE bapisditmx_tt,
*              lt_sch_in    TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
*              lt_sch_inx   TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY,
*              lt_return    TYPE bapiret2_tab,
*              lv_new_vbeln TYPE vbeln_va.
*
*        CLEAR: ls_head_in, ls_head_inx, ls_logic, lt_items_in, lt_items_inx, lt_sch_in, lt_sch_inx, lt_return, lv_new_vbeln.
*
*        ls_head_in-doc_type    = gc_auart.
*        ls_head_in-sales_org   = ls_ref_hdr-vkorg.
*        ls_head_in-distr_chan  = ls_ref_hdr-vtweg.
*        ls_head_in-division    = ls_ref_hdr-spart.
*        ls_head_in-ref_doc     = lv_refdoc.
*        ls_head_in-refdoc_cat  = 'G'.
*        ls_head_in-req_date_h  = ls_input-deliverydate.
*        ls_head_in-purch_no_c  = ls_input-customerref.
*
*        "Set language if component exists
*        FIELD-SYMBOLS <f> TYPE any.
*        ASSIGN COMPONENT 'LANGU' OF STRUCTURE ls_head_in TO <f>.
*        IF sy-subrc = 0. <f> = lv_bapi_langu. ENDIF.
*
*        ls_head_inx-updateflag = 'I'.
*        ls_head_inx-doc_type   = 'X'.
*        ls_head_inx-sales_org  = 'X'.
*        ls_head_inx-distr_chan = 'X'.
*        ls_head_inx-division   = 'X'.
*        ls_head_inx-ref_doc    = 'X'.
*        ls_head_inx-refdoc_cat = 'X'.
*        ls_head_inx-req_date_h = 'X'.
*        ls_head_inx-purch_no_c = 'X'.
*
*        ASSIGN COMPONENT 'LANGU' OF STRUCTURE ls_head_inx TO <f>.
*        IF sy-subrc = 0. <f> = 'X'. ENDIF.
*
*        ls_logic-cond_handl = 'X'.
*
*        LOOP AT lt_agg INTO DATA(ls_agg).
*
*          DATA: ls_item_in  TYPE bapisditm,
*                ls_item_inx TYPE bapisditmx,
*                ls_sch1     TYPE bapischdl,
*                ls_sch1x    TYPE bapischdlx.
*
*          CLEAR: ls_item_in, ls_item_inx, ls_sch1, ls_sch1x.
*
*          ls_item_in-itm_number = ls_agg-posnr.
*          ls_item_in-material   = ls_agg-matnr.
*          ls_item_in-target_qty = ls_agg-qty.
*          ls_item_in-target_qu  = ls_agg-meins.
*          ls_item_in-ref_doc    = lv_refdoc.
*          ls_item_in-ref_doc_it = ls_agg-posnr.
*          ls_item_in-ref_doc_ca = 'G'.
*          IF ls_agg-werks IS NOT INITIAL.
*            ls_item_in-plant = ls_agg-werks.
*          ENDIF.
*          APPEND ls_item_in TO lt_items_in.
*
*          ls_item_inx-itm_number = ls_agg-posnr.
*          ls_item_inx-updateflag = 'I'.
*          ls_item_inx-material   = 'X'.
*          ls_item_inx-target_qty = 'X'.
*          ls_item_inx-target_qu  = 'X'.
*          ls_item_inx-ref_doc    = 'X'.
*          ls_item_inx-ref_doc_it = 'X'.
*          ls_item_inx-ref_doc_ca = 'X'.
*          IF ls_item_in-plant IS NOT INITIAL.
*            ls_item_inx-plant = 'X'.
*          ENDIF.
*          APPEND ls_item_inx TO lt_items_inx.
*
*          ls_sch1-itm_number = ls_agg-posnr.
*          ls_sch1-sched_line = '0001'.
*          ls_sch1-req_qty    = ls_agg-qty.
*          ls_sch1-req_date   = ls_input-deliverydate.
*          APPEND ls_sch1 TO lt_sch_in.
*
*          ls_sch1x-itm_number = ls_agg-posnr.
*          ls_sch1x-sched_line = '0001'.
*          ls_sch1x-updateflag = 'I'.
*          ls_sch1x-req_qty    = 'X'.
*          ls_sch1x-req_date   = 'X'.
*          APPEND ls_sch1x TO lt_sch_inx.
*
*        ENDLOOP.
*
*        CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
*          EXPORTING
*            order_header_in      = ls_head_in
*            order_header_inx     = ls_head_inx
*            logic_switch         = ls_logic
*          IMPORTING
*            salesdocument        = lv_new_vbeln
*          TABLES
*            return               = lt_return
*            order_items_in       = lt_items_in
*            order_items_inx      = lt_items_inx
*            order_schedules_in   = lt_sch_in
*            order_schedules_inx  = lt_sch_inx
*            order_partners       = lt_partners_dat2.
*
*        IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ) OR lv_new_vbeln IS INITIAL.
*          LOOP AT lt_return INTO DATA(ls_e1) WHERE type CA 'EA'.
*            APPEND VALUE #( %msg = new_message(
*              id       = ls_e1-id
*              number   = ls_e1-number
*              v1       = ls_e1-message_v1
*              v2       = ls_e1-message_v2
*              v3       = ls_e1-message_v3
*              v4       = ls_e1-message_v4
*              severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*          ENDLOOP.
*          failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*          RETURN.
*        ENDIF.
*
*        result = VALUE #(
*          ( %param = VALUE #(
*              techkey   = 'X'
*              vbeln     = lv_new_vbeln
*              _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (DAT2)' ) )
*          ) )
*        ).
*        RETURN.
*
*      ENDIF.
*
*      "=========================================================
*      " Branch B: VBTYP <> 'C' (e.g. 'L') -> use generic BAPI
*      "=========================================================
*      DATA lv_busobj TYPE swo_objtyp.
*      CLEAR lv_busobj.
*
*      CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
*        EXPORTING
*          i_document_type   = lv_vbtyp
*        IMPORTING
*          e_business_object = lv_busobj.
*
*      IF lv_busobj IS INITIAL.
*        APPEND VALUE #( %msg = new_message(
*          id       = '00'
*          number   = '001'
*          v1       = |Cannot determine BOR object for VBTYP { lv_vbtyp }|
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*        failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*        RETURN.
*      ENDIF.
*
*      DATA: ls_head_gen TYPE bapisdhead,
*            lt_item_gen TYPE STANDARD TABLE OF bapiitemin WITH DEFAULT KEY,
*            ls_ret1     TYPE bapireturn1,
*            lv_new_doc  TYPE vbeln_va.
*
*      CLEAR: ls_head_gen, lt_item_gen, ls_ret1, lv_new_doc.
*
*      ls_head_gen-doc_type   = gc_auart.
*      ls_head_gen-sales_org  = ls_ref_hdr-vkorg.
*      ls_head_gen-distr_chan = ls_ref_hdr-vtweg.
*      ls_head_gen-division   = ls_ref_hdr-spart.
*      ls_head_gen-ref_doc    = lv_refdoc.
*      ls_head_gen-ref_doc_ca = 'G'.
*      ls_head_gen-req_date_h = ls_input-deliverydate.
*      ls_head_gen-purch_no_c = ls_input-customerref.
*
*      "Set language if component exists
*      FIELD-SYMBOLS <g> TYPE any.
*      ASSIGN COMPONENT 'LANGU' OF STRUCTURE ls_head_gen TO <g>.
*      IF sy-subrc = 0. <g> = lv_bapi_langu. ENDIF.
*
*      LOOP AT lt_agg INTO DATA(ls_agg2).
*        APPEND VALUE bapiitemin(
*          itm_number = ls_agg2-posnr
*          material   = ls_agg2-matnr
*          target_qty = ls_agg2-qty
*          target_qu  = ls_agg2-meins
*          ref_doc    = lv_refdoc
*          ref_doc_it = ls_agg2-posnr
*          ref_doc_ca = 'G'
*          plant      = ls_agg2-werks ) TO lt_item_gen.
*      ENDLOOP.
*
*      CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
*        EXPORTING
*          order_header_in = ls_head_gen
*          business_object = lv_busobj
*          without_commit  = abap_true
*        IMPORTING
*          salesdocument   = lv_new_doc
*          return          = ls_ret1
*        TABLES
*          order_items_in  = lt_item_gen
*          order_partners  = lt_partners_gen.
*
*      IF lv_new_doc IS INITIAL OR ls_ret1-type CA 'EAX'.
*        APPEND VALUE #( %msg = new_message(
*          id       = ls_ret1-id
*          number   = ls_ret1-number
*          v1       = ls_ret1-message_v1
*          v2       = ls_ret1-message_v2
*          v3       = ls_ret1-message_v3
*          v4       = ls_ret1-message_v4
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*        failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*        RETURN.
*      ENDIF.
*
*      result = VALUE #(
*        ( %param = VALUE #(
*            techkey   = 'X'
*            vbeln     = lv_new_doc
*            _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (GENERIC)' ) )
*        ) )
*      ).
*
**    FINALLY.
*      "Restore original language
*      SET LOCALE LANGUAGE lv_old_langu.
*    ENDTRY.
*
*ENDMETHOD.

*METHOD createlanf.
*
*  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.
*
*  "1) Input
*  READ TABLE keys INTO DATA(ls_key) INDEX 1.
*  DATA(ls_input) = ls_key-%param.
*
*  DATA lv_contract TYPE vbeln_va.
*  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.
*
*  "2) Contract header
*  SELECT SINGLE * FROM vbak
*    WHERE vbeln = @lv_contract
*    INTO @DATA(ls_vbak).
*
*  IF sy-subrc <> 0.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00' number = '001'
*      v1       = |Contract { lv_contract } not found|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "3) Contract items
*  TYPES: BEGIN OF ty_ctr_item,
*           posnr TYPE posnr_va,
*           matnr TYPE matnr,
*           werks TYPE werks_d,
*           vrkme TYPE vrkme,
*           pstyv TYPE pstyv,
*           netpr TYPE netpr,
*           kpein TYPE kpein,
*           kmein TYPE kmein,
*         END OF ty_ctr_item.
*
*  DATA lt_ctr_items TYPE STANDARD TABLE OF ty_ctr_item WITH EMPTY KEY.
*
*  SELECT posnr, matnr, werks, vrkme, pstyv, netpr, kpein, kmein
*    FROM vbap
*    WHERE vbeln = @lv_contract
*    INTO TABLE @lt_ctr_items.
*
*  IF lt_ctr_items IS INITIAL.
*    APPEND VALUE #( %msg = new_message(
*      id       = '00' number = '001'
*      v1       = |No items found in contract { lv_contract }|
*      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "4) Aggregate input positions by POSNR (we derive POSNR by MATNR mapping)
*  TYPES: BEGIN OF ty_agg,
*           posnr TYPE posnr_va,
*           qty   TYPE kwmeng,
*         END OF ty_agg.
*
*  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
*  FIELD-SYMBOLS <agg> TYPE ty_agg.
*
*  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.
*
*    DATA lv_matnr  TYPE matnr.
*    lv_matnr = ls_pos-matnr.
*    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
*      EXPORTING input  = lv_matnr
*      IMPORTING output = lv_matnr.
*
*    "Map MATNR -> contract item
*    READ TABLE lt_ctr_items INTO DATA(ls_ctr) WITH KEY matnr = lv_matnr.
*    IF sy-subrc <> 0.
*      APPEND VALUE #( %msg = new_message(
*        id       = '00' number = '001'
*        v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      CONTINUE.
*    ENDIF.
*
*    "Unit check (optional but recommended)
*    IF ls_pos-meins IS NOT INITIAL AND ls_ctr-vrkme IS NOT INITIAL AND ls_pos-meins <> ls_ctr-vrkme.
*      APPEND VALUE #( %msg = new_message(
*        id       = '00' number = '001'
*        v1       = |UoM mismatch for { ls_pos-matnr }: input { ls_pos-meins } <> contract { ls_ctr-vrkme }|
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      CONTINUE.
*    ENDIF.
*
*    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
*    IF sy-subrc = 0.
*      <agg>-qty = <agg>-qty + ls_pos-menge.
*    ELSE.
*      INSERT VALUE ty_agg( posnr = ls_ctr-posnr qty = ls_pos-menge ) INTO TABLE lt_agg.
*    ENDIF.
*
*  ENDLOOP.
*
*  IF reported-lanfroot IS NOT INITIAL OR lt_agg IS INITIAL.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "5) Partners from contract
*  DATA lt_partners TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.
*
*  SELECT parvw, kunnr
*    FROM vbpa
*    WHERE vbeln = @lv_contract
*      AND kunnr <> ''
*    INTO TABLE @DATA(lt_vbpa).
*
*  lt_partners = VALUE #( FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
*
*  IF NOT line_exists( lt_partners[ partn_role = 'AG' ] ) AND ls_vbak-kunnr IS NOT INITIAL.
*    APPEND VALUE bapipartnr( partn_role = 'AG' partn_numb = ls_vbak-kunnr ) TO lt_partners.
*  ENDIF.
*
*  "6) Build BAPI header (CREATEFROMDATA1)
*  DATA ls_head  TYPE bapisdhead1.
*  DATA ls_headx TYPE bapisdhead1x.
*
*  CLEAR: ls_head, ls_headx.
*
*  ls_head-doc_type   = gc_auart.
*  ls_head-sales_org  = ls_vbak-vkorg.
*  ls_head-distr_chan = ls_vbak-vtweg.
*  ls_head-division   = ls_vbak-spart.
*  ls_head-req_date_h = ls_input-deliverydate.
*  ls_head-purch_no_c = ls_input-customerref.
*
*  ls_head-ref_doc    = lv_contract.
*  ls_head-ref_doc_cat = 'G'.
*
*  ls_headx-updateflag = 'I'.
*  ls_headx-doc_type   = 'X'.
*  ls_headx-sales_org  = 'X'.
*  ls_headx-distr_chan = 'X'.
*  ls_headx-division   = 'X'.
*  ls_headx-req_date_h = 'X'.
*  ls_headx-purch_no_c = 'X'.
*  ls_headx-ref_doc    = 'X'.
*  ls_headx-ref_doc_cat = 'X'.
*
*  "7) Build items + schedules + conditions
*  DATA lt_items    TYPE STANDARD TABLE OF bapisditem WITH DEFAULT KEY.
*  DATA lt_itemsx   TYPE STANDARD TABLE OF bapisditemx WITH DEFAULT KEY.
*  DATA lt_sched    TYPE STANDARD TABLE OF bapischedule WITH DEFAULT KEY.
*  DATA lt_schedx   TYPE STANDARD TABLE OF bapischedulex WITH DEFAULT KEY.
*  DATA lt_cond     TYPE STANDARD TABLE OF bapicondition WITH DEFAULT KEY.
*  DATA lt_return   TYPE bapiret2_tab.
*
*  CLEAR: lt_items, lt_itemsx, lt_sched, lt_schedx, lt_cond, lt_return.
*
*  LOOP AT lt_agg INTO DATA(ls_agg).
*
*    READ TABLE lt_ctr_items INTO ls_ctr WITH KEY posnr = ls_agg-posnr.
*    IF sy-subrc <> 0.
*      CONTINUE.
*    ENDIF.
*
*    "ITEM
*    DATA ls_item  TYPE bapisditem.
*    DATA ls_itemx TYPE bapisditemx.
*    CLEAR: ls_item, ls_itemx.
*
*    ls_item-itm_number = ls_ctr-posnr.
*    ls_item-material   = ls_ctr-matnr.
*    ls_item-plant      = ls_ctr-werks.
**    ls_item-item_categ = ls_ctr-pstyv.
*    ls_item-target_qty = ls_agg-qty.
*    ls_item-target_qu  = ls_ctr-vrkme.
*
*    ls_item-ref_doc    = lv_contract.
*    ls_item-ref_doc_it = ls_ctr-posnr.
*    ls_item-ref_doc_ca = 'G'.
*
*    APPEND ls_item TO lt_items.
*
*    ls_itemx-itm_number = ls_ctr-posnr.
*    ls_itemx-updateflag = 'I'.
*    ls_itemx-material   = 'X'.
*    ls_itemx-plant      = 'X'.
**    ls_itemx-item_categ = 'X'.
*    ls_itemx-target_qty = 'X'.
*    ls_itemx-target_qu  = 'X'.
*    ls_itemx-ref_doc    = 'X'.
*    ls_itemx-ref_doc_it = 'X'.
*    ls_itemx-ref_doc_ca = 'X'.
*    APPEND ls_itemx TO lt_itemsx.
*
*    "SCHEDULE (makes Zielmenge consistent)
*    DATA ls_sc  TYPE bapischedule.
*    DATA ls_scx TYPE bapischedulex.
*    CLEAR: ls_sc, ls_scx.
*
*    ls_sc-itm_number = ls_ctr-posnr.
*    ls_sc-sched_line = '0001'.
*    ls_sc-req_qty    = ls_agg-qty.
*    ls_sc-req_date   = ls_input-deliverydate.
*    APPEND ls_sc TO lt_sched.
*
*    ls_scx-itm_number = ls_ctr-posnr.
*    ls_scx-sched_line = '0001'.
*    ls_scx-updateflag = 'I'.
*    ls_scx-req_qty    = 'X'.
*    ls_scx-req_date   = 'X'.
*    APPEND ls_scx TO lt_schedx.
*
*    "CONDITION: pass net price from contract (fix Nettopreis/Nettowert incompletion)
*    IF ls_ctr-netpr IS INITIAL.
*      APPEND VALUE #( %msg = new_message(
*        id       = '00' number = '001'
*        v1       = |Contract item { ls_ctr-posnr } has no NETPR -> pricing must be maintained|
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      CONTINUE.
*    ENDIF.
*
*    DATA lv_pu TYPE kpein.
*    lv_pu = ls_ctr-kpein.
*    IF lv_pu IS INITIAL.
*      lv_pu = 1.
*    ENDIF.
*
*    APPEND VALUE bapicondition(
*      itm_number = ls_ctr-posnr
*      cond_st_no = '010'
*      cond_count = '01'
*      cond_type  = 'PR00'            "Adjust if your system uses ZPR0, etc.
*      cond_value = ls_ctr-netpr
*      currency   = ls_vbak-waerk
*      cond_p_unt = lv_pu
*      cond_unit  = COND #( WHEN ls_ctr-kmein IS INITIAL THEN ls_ctr-vrkme ELSE ls_ctr-kmein )
*    ) TO lt_cond.
*
*  ENDLOOP.
*
*  IF reported-lanfroot IS NOT INITIAL.
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "8) Force German locale (your texts exist only in DE)
*  DATA(lv_old_langu) = sy-langu.
*  SET LOCALE LANGUAGE 'D'.
*
*  DATA lv_new_doc TYPE bapivbeln-vbeln.
*
*  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA1'
*    EXPORTING
*      sales_header_in   = ls_head
*      sales_header_inx  = ls_headx
*      int_number_assignment = space
*    IMPORTING
*      salesdocument_ex  = lv_new_doc
*    TABLES
*      return              = lt_return
*      sales_items_in      = lt_items
*      sales_items_inx     = lt_itemsx
*      sales_partners      = lt_partners
*      sales_schedules_in  = lt_sched
*      sales_schedules_inx = lt_schedx
*      sales_conditions_in = lt_cond.
*
*  SET LOCALE LANGUAGE lv_old_langu.
*
*  "9) Handle errors
*  IF lv_new_doc IS INITIAL OR line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
*
*    LOOP AT lt_return INTO DATA(ls_r) WHERE type CA 'EA'.
*      APPEND VALUE #( %msg = new_message(
*        id       = ls_r-id
*        number   = ls_r-number
*        v1       = ls_r-message_v1
*        v2       = ls_r-message_v2
*        v3       = ls_r-message_v3
*        v4       = ls_r-message_v4
*        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*    ENDLOOP.
*
*    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    RETURN.
*  ENDIF.
*
*  "Success
*  result = VALUE #(
*    ( %param = VALUE #(
*        techkey   = 'X'
*        vbeln     = lv_new_doc
*        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Created successfully' ) )
*    ) )
*  ).
*
*ENDMETHOD.


METHOD createlanf.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA lv_contract TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  DATA lt_return TYPE bapiret2_tab.
  DATA lv_new_so TYPE vbeln_va.

  DATA lv_sysmsg TYPE char60.
  DATA lv_commmsg TYPE char60.

  "RFC loopback call -> separate LUW -> no RAP update-task dump
  CALL FUNCTION 'Z_LANF_CREATE_DMREQ_RFC'
    DESTINATION 'NONE'
    EXPORTING
      iv_contract     = lv_contract
      iv_deliverydate = ls_input-deliverydate
      iv_customerref  = ls_input-customerref
      iv_auart        = 'ZLRA'
    IMPORTING
      ev_vbeln        = lv_new_so
    TABLES
      it_positions    = ls_input-_positions
      et_return       = lt_return
    EXCEPTIONS
      system_failure        = 1 MESSAGE lv_sysmsg
      communication_failure = 2 MESSAGE lv_commmsg
      OTHERS                = 3.

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |RFC call failed: { cond #( when sy-subrc = 1 then lv_sysmsg when sy-subrc = 2 then lv_commmsg else |SUBRC { sy-subrc }| ) }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Map BAPI return to RAP messages
  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_r) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id       = ls_r-id
        number   = ls_r-number
        v1       = ls_r-message_v1
        v2       = ls_r-message_v2
        v3       = ls_r-message_v3
        v4       = ls_r-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Success -> return VBELN
  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_so
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Created successfully' ) )
    ) )
  ).

ENDMETHOD.




METHOD attachdocument.

  DATA: ls_input         TYPE zi_attach_input,
        lt_return_attach TYPE bapiret2_tab,
        lv_vbtyp         TYPE vbak-vbtyp,
        lv_busobj        TYPE swo_objtyp.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  ls_input = CORRESPONDING #( ls_key-%param ).

  DATA(lv_vbeln) = |{ ls_input-vbeln ALPHA = IN }|.

  " Determine SD doc category (VBTYP)
  SELECT SINGLE vbtyp FROM vbak
    WHERE vbeln = @lv_vbeln
    INTO @lv_vbtyp.

  IF sy-subrc <> 0.
    APPEND VALUE #( type = 'E' id = '00' number = '001'
      message = |Sales document { lv_vbeln } not found| ) TO lt_return_attach.
  ELSE.
    " Determine correct BOR object for that category (avoids BUS2032/L issue)
    CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
      EXPORTING
        i_document_type   = lv_vbtyp
      IMPORTING
        e_business_object = lv_busobj.
    " (This FM is commonly used to map SD category -> BOR object) :contentReference[oaicite:3]{index=3}

    " Attach URL as GOS/BDS link
    CALL FUNCTION 'BDS_BUSINESSDOCUMENT_CRE_O_URL'
      EXPORTING
        classname       = lv_busobj
        classtype       = 'BO'
        object_key      = lv_vbeln
        url             = ls_input-url
        url_description = ls_input-title
      EXCEPTIONS
        nothing_found   = 1
        parameter_error = 2
        not_allowed     = 3
        error_kpro      = 4
        internal_error  = 5
        OTHERS          = 6.

    IF sy-subrc <> 0.
      APPEND VALUE #( type = 'E' id = sy-msgid number = sy-msgno
        message_v1 = sy-msgv1 message_v2 = sy-msgv2 message_v3 = sy-msgv3 message_v4 = sy-msgv4
        message = |Attach URL failed (SY-SUBRC={ sy-subrc })| ) TO lt_return_attach.
    ELSE.
      APPEND VALUE #( type = 'S' id = '00' number = '000'
        message = |URL attached successfully| ) TO lt_return_attach.
    ENDIF.
  ENDIF.

  " Return VBELN + messages
  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_vbeln
        _messages = VALUE #(
          FOR r IN lt_return_attach (
            techkey = 'X'
            msgid   = r-id
            msgno   = r-number
            msgty   = r-type
            msgv1   = r-message_v1
            msgv2   = r-message_v2
            msgv3   = r-message_v3
            msgv4   = r-message_v4 ) ) ) ) ).

ENDMETHOD.

METHOD map_positions_to_contract.

  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @iv_contract_vbeln
    INTO TABLE @DATA(lt_contract_pos).

  LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<pos>) WHERE menge > 0.

    DATA(lv_matnr) = <pos>-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_contract_pos ASSIGNING FIELD-SYMBOL(<contr_pos>)
      WITH KEY matnr = lv_matnr.

    IF sy-subrc = 0.
      APPEND VALUE bapisditm(
        itm_number = <contr_pos>-posnr
        material   = lv_matnr
        target_qty = <pos>-menge
        target_qu  = <pos>-meins
        ref_doc    = iv_contract_vbeln
        ref_doc_it = <contr_pos>-posnr
        ref_doc_ca = 'G' ) TO ct_order_items.

      APPEND VALUE bapisditmx(
        itm_number = <contr_pos>-posnr
        updateflag = 'I'
        material   = 'X'
        target_qty = 'X'
        target_qu  = 'X'
        ref_doc    = 'X'
        ref_doc_it = 'X'
        ref_doc_ca = 'X' ) TO ct_order_itemsx.
    ELSE.
      APPEND VALUE bapiret2(
        type   = 'E'
        id     = '00'
        number = '001'
        message = |No position for MATNR { lv_matnr } in contract { iv_contract_vbeln }| ) TO ct_return.
    ENDIF.

  ENDLOOP.

ENDMETHOD.



ENDCLASS.

CLASS lsc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.

    METHODS finalize REDEFINITION.

    METHODS check_before_save REDEFINITION.

    METHODS save REDEFINITION.

    METHODS cleanup REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_zi_lanf_root IMPLEMENTATION.

  METHOD finalize.
  ENDMETHOD.

  METHOD check_before_save.
  ENDMETHOD.

  METHOD save.
  ENDMETHOD.

  METHOD cleanup.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.




FUNCTION Z_LANF_CREATE_DMREQ_RFC.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IV_CONTRACT) TYPE  VBELN_VA
*"     VALUE(IV_DELIVERYDAT) TYPE  DATS
*"     VALUE(IV_CUSTOMERREF) TYPE  BSTKD
*"     VALUE(IV_AUART) TYPE  AUART DEFAULT 'ZLRA'
*"  EXPORTING
*"     VALUE(EV_VBELN) TYPE  VBELN_VA
*"  TABLES
*"      IT_POSITIONS TYPE  ZLANF_POS_IN_TT
*"      ET_RETURN TYPE  BAPIRET2_TAB
*"----------------------------------------------------------------------

CLEAR: ev_vbeln, et_return[].

  "Run SD in German (your system has TSPAT only DE)
  DATA(lv_old_langu) = sy-langu.
  SET LOCALE LANGUAGE 'D'.

  "1) Contract header
  SELECT SINGLE * FROM vbak
    WHERE vbeln = @iv_contract
    INTO @DATA(ls_vbak).

  IF sy-subrc <> 0.
    APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
      message = |Contract { iv_contract } not found| ) TO et_return.
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "2) Contract items for mapping (POSNR/MATNR/UoM)
  TYPES: BEGIN OF ty_ctr,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           vrkme TYPE vrkme,
         END OF ty_ctr.
  DATA lt_ctr TYPE STANDARD TABLE OF ty_ctr WITH EMPTY KEY.

  SELECT posnr, matnr, vrkme
    FROM vbap
    WHERE vbeln = @iv_contract
    INTO TABLE @lt_ctr.

  IF lt_ctr IS INITIAL.
    APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
      message = |No items in contract { iv_contract }| ) TO et_return.
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "3) Aggregate positions by contract POSNR (prevents duplicates / M_/011)
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,
           qty   TYPE kwmeng,
           meins TYPE meins,
           matnr TYPE matnr,
         END OF ty_agg.
  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
  FIELD-SYMBOLS <agg> TYPE ty_agg.

  LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<p>) WHERE menge > 0.

    DATA lv_matnr TYPE matnr.
    lv_matnr = <p>-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    "Detect ambiguity: same material multiple times in contract
    DATA lv_hits TYPE i VALUE 0.
    DATA ls_ctr  TYPE ty_ctr.
    LOOP AT lt_ctr INTO ls_ctr WHERE matnr = lv_matnr.
      lv_hits += 1.
      IF lv_hits = 1.
        "keep first hit in ls_ctr
      ENDIF.
    ENDLOOP.

    IF lv_hits = 0.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |Material { <p>-matnr } not found in contract { iv_contract }| ) TO et_return.
      CONTINUE.
*    ELSEIF lv_hits > 1.
*      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
*        message = |Material { <p>-matnr } occurs multiple times in contract { iv_contract } -> ambiguous| ) TO et_return.
*      CONTINUE.
    ENDIF.

    "UoM check
    IF <p>-meins IS NOT INITIAL AND ls_ctr-vrkme IS NOT INITIAL AND <p>-meins <> ls_ctr-vrkme.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |UoM mismatch for { <p>-matnr }: { <p>-meins } <> contract { ls_ctr-vrkme }| ) TO et_return.
      CONTINUE.
    ENDIF.

    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
    IF sy-subrc = 0.
      <agg>-qty = <agg>-qty + <p>-menge.
    ELSE.
      INSERT VALUE ty_agg(
        posnr = ls_ctr-posnr
        qty   = <p>-menge
        meins = ls_ctr-vrkme
        matnr = lv_matnr ) INTO TABLE lt_agg.
    ENDIF.

  ENDLOOP.

  IF line_exists( et_return[ type = 'E' ] ) OR lt_agg IS INITIAL.
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "4) Build BAPI (CREATEFROMDATA1) structures
  DATA ls_head   TYPE bapisdhead1.
  DATA ls_headx  TYPE bapisdhead1x.
  DATA lt_items  TYPE STANDARD TABLE OF bapisditem WITH DEFAULT KEY.
  DATA lt_itemsx TYPE STANDARD TABLE OF bapisditemx WITH DEFAULT KEY.
  DATA lt_sched  TYPE STANDARD TABLE OF bapischedule WITH DEFAULT KEY.
  DATA lt_schedx TYPE STANDARD TABLE OF bapischedulex WITH DEFAULT KEY.
  DATA lt_part   TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.
  DATA lt_cond   TYPE STANDARD TABLE OF bapicondition WITH DEFAULT KEY.

  CLEAR: ls_head, ls_headx.

  ls_head-doc_type   = iv_auart.
  ls_head-sales_org  = ls_vbak-vkorg.
  ls_head-distr_chan = ls_vbak-vtweg.
  ls_head-division   = ls_vbak-spart.
  ls_head-req_date_h = iv_deliverydat.
  ls_head-purch_no_c = iv_customerref.
  ls_head-ref_doc    = iv_contract.
  ls_head-ref_doc_cat = 'G'.

  ls_headx-updateflag = 'I'.
  ls_headx-doc_type   = 'X'.
  ls_headx-sales_org  = 'X'.
  ls_headx-distr_chan = 'X'.
  ls_headx-division   = 'X'.
  ls_headx-req_date_h = 'X'.
  ls_headx-purch_no_c = 'X'.
  ls_headx-ref_doc    = 'X'.
  ls_headx-ref_doc_cat = 'X'.

  "Partners from contract
  SELECT parvw, kunnr
    FROM vbpa
    WHERE vbeln = @iv_contract AND kunnr <> ''
    INTO TABLE @DATA(lt_vbpa).

  lt_part = VALUE #( FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).
  IF NOT line_exists( lt_part[ partn_role = 'AG' ] ) AND ls_vbak-kunnr IS NOT INITIAL.
    APPEND VALUE bapipartnr( partn_role = 'AG' partn_numb = ls_vbak-kunnr ) TO lt_part.
  ENDIF.

  "Conditions: pick PR00 from contract pricing if possible; else fallback to VBAP-NETPR
  DATA lv_knumv TYPE knumv.
  lv_knumv = ls_vbak-knumv.

  LOOP AT lt_agg INTO DATA(ls_agg).

    APPEND VALUE bapisditem(
      itm_number = ls_agg-posnr
      material   = ls_agg-matnr
      target_qty = ls_agg-qty
      target_qu  = ls_agg-meins
      ref_doc    = iv_contract
      ref_doc_it = ls_agg-posnr
      ref_doc_ca = 'G'
    ) TO lt_items.

    APPEND VALUE bapisditemx(
      itm_number = ls_agg-posnr
      updateflag = 'I'
      material   = 'X'
      target_qty = 'X'
      target_qu  = 'X'
      ref_doc    = 'X'
      ref_doc_it = 'X'
      ref_doc_ca = 'X'
    ) TO lt_itemsx.

    APPEND VALUE bapischedule(
      itm_number = ls_agg-posnr
      sched_line = '0001'
      req_qty    = ls_agg-qty
      req_date   = iv_deliverydat
    ) TO lt_sched.

    APPEND VALUE bapischedulex(
      itm_number = ls_agg-posnr
      sched_line = '0001'
      updateflag = 'I'
      req_qty    = 'X'
      req_date   = 'X'
    ) TO lt_schedx.

    "Try get PR00 (or first active price condition) from PRCD_ELEMENTS
    DATA: lv_kschl TYPE kschl VALUE 'PR00',
          lv_kbetr TYPE kbetr_kond,
          lv_waers TYPE waers,
          lv_kpein TYPE kpein,
          lv_kmein TYPE kmein.

    CLEAR: lv_kbetr, lv_waers, lv_kpein, lv_kmein.

    IF lv_knumv IS NOT INITIAL.
      SELECT SINGLE kschl, kbetr, waers, kpein, kmein
        FROM prcd_elements
        WHERE knumv = @lv_knumv
          AND kposn = @ls_agg-posnr
          AND kinak = ''
          AND kstat = ''
          AND kschl = 'PR00'
        INTO @DATA(ls_pr00).

      IF sy-subrc = 0.
        lv_kschl = ls_pr00-kschl.
        lv_kbetr = ls_pr00-kbetr.
        lv_waers = ls_pr00-waers.
        lv_kpein = ls_pr00-kpein.
        lv_kmein = ls_pr00-kmein.
      ENDIF.
    ENDIF.

    "Fallback: VBAP-NETPR if PRCD_ELEMENTS not found
    IF lv_kbetr IS INITIAL.
      SELECT SINGLE netpr, kpein, kmein
        FROM vbap
        WHERE vbeln = @iv_contract AND posnr = @ls_agg-posnr
        INTO @DATA(ls_vbap_price).
      lv_kbetr = ls_vbap_price-netpr.
      lv_kpein = COND #( WHEN ls_vbap_price-kpein IS INITIAL THEN 1 ELSE ls_vbap_price-kpein ).
      lv_kmein = COND #( WHEN ls_vbap_price-kmein IS INITIAL THEN ls_agg-meins ELSE ls_vbap_price-kmein ).
      lv_waers = ls_vbak-waerk.
    ENDIF.

    IF lv_kbetr IS INITIAL.
      APPEND VALUE bapiret2( type = 'E' id = '00' number = '001'
        message = |No price found for item { ls_agg-posnr } in contract -> cannot satisfy Nettopreis/Nettowert| ) TO et_return.
      CONTINUE.
    ENDIF.

    APPEND VALUE bapicondition(
      itm_number = ls_agg-posnr
      cond_st_no = '010'
      cond_count = '01'
      cond_type  = lv_kschl
      cond_value = lv_kbetr
      currency   = lv_waers
      cond_p_unt = COND #( WHEN lv_kpein IS INITIAL THEN 1 ELSE lv_kpein )
      cond_unit  = COND #( WHEN lv_kmein IS INITIAL THEN ls_agg-meins ELSE lv_kmein )
    ) TO lt_cond.

  ENDLOOP.

  IF line_exists( et_return[ type = 'E' ] ).
    SET LOCALE LANGUAGE lv_old_langu.
    RETURN.
  ENDIF.

  "5) Call BAPI in this RFC session (allowed to use update-task)
  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA1'
    EXPORTING
      sales_header_in   = ls_head
      sales_header_inx  = ls_headx
      int_number_assignment = space
    IMPORTING
      salesdocument_ex  = ev_vbeln
    TABLES
      return              = et_return
      sales_items_in      = lt_items
      sales_items_inx     = lt_itemsx
      sales_partners      = lt_part
      sales_schedules_in  = lt_sched
      sales_schedules_inx = lt_schedx
      sales_conditions_in = lt_cond.

  IF ev_vbeln IS INITIAL OR line_exists( et_return[ type = 'E' ] ) OR line_exists( et_return[ type = 'A' ] ).
    CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
  ELSE.
    CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
      EXPORTING wait = abap_true.
  ENDIF.

  SET LOCALE LANGUAGE lv_old_langu.

ENDFUNCTION.
