METHOD createlanf.

  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "Debit Memo Request / your doc type

  "------------------------------------------------------------
  " 1) Read action input
  "------------------------------------------------------------
  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA(lv_contract) TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "------------------------------------------------------------
  " 2) Contract header
  "------------------------------------------------------------
  SELECT SINGLE * FROM vbak
    WHERE vbeln = @lv_contract
    INTO @DATA(ls_vbak).

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |Contract { lv_contract } not found|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "------------------------------------------------------------
  " 3) Contract items (include pricing-related fields)
  "------------------------------------------------------------
  TYPES: BEGIN OF ty_ctr_item,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           werks TYPE werks_d,
           vrkme TYPE vrkme,
           pstyv TYPE pstyv,
           netpr TYPE netpr,
           kpein TYPE kpein,
           kmein TYPE kmein,
         END OF ty_ctr_item.

  DATA lt_ctr_items TYPE STANDARD TABLE OF ty_ctr_item WITH EMPTY KEY.

  SELECT posnr, matnr, werks, vrkme, pstyv, netpr, kpein, kmein
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

  "------------------------------------------------------------
  " 4) Aggregate input positions by contract POSNR (avoid duplicates)
  "------------------------------------------------------------
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,
           qty   TYPE kwmeng,
           meins TYPE vrkme,
           matnr TYPE matnr,
         END OF ty_agg.

  DATA lt_agg TYPE HASHED TABLE OF ty_agg WITH UNIQUE KEY posnr.
  FIELD-SYMBOLS <agg> TYPE ty_agg.

  LOOP AT ls_input-_positions INTO DATA(ls_pos) WHERE menge > 0.

    "Normalize MATNR
    DATA(lv_matnr) TYPE matnr.
    lv_matnr = ls_pos-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    "Find contract items for this material
    DATA(lt_hit) = VALUE ty_ctr_item_tt( FOR x IN lt_ctr_items WHERE ( matnr = lv_matnr ) ( x ) ).
    "If your release doesn't support the above inline filter, replace with a LOOP/APPEND.
    "To stay safe on all systems, we do it without that helper:

    CLEAR lt_hit.
    LOOP AT lt_ctr_items INTO DATA(ls_ctr_scan) WHERE matnr = lv_matnr.
      APPEND ls_ctr_scan TO lt_hit.
    ENDLOOP.

    IF lines( lt_hit ) = 0.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    IF lines( lt_hit ) > 1.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Material { ls_pos-matnr } occurs multiple times in contract { lv_contract } -> mapping ambiguous|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    READ TABLE lt_hit INTO DATA(ls_ctr) INDEX 1.

    "Unit check (important to avoid SD issues)
    IF ls_pos-meins IS NOT INITIAL AND ls_ctr-vrkme IS NOT INITIAL AND ls_pos-meins <> ls_ctr-vrkme.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |UoM mismatch for { ls_pos-matnr }: input { ls_pos-meins } <> contract { ls_ctr-vrkme }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
    IF sy-subrc = 0.
      <agg>-qty = <agg>-qty + ls_pos-menge.
    ELSE.
      INSERT VALUE ty_agg(
        posnr = ls_ctr-posnr
        qty   = ls_pos-menge
        meins = ls_ctr-vrkme
        matnr = lv_matnr ) INTO TABLE lt_agg.
    ENDIF.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL OR lt_agg IS INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "------------------------------------------------------------
  " 5) Partners from contract (ensure at least AG exists)
  "------------------------------------------------------------
  DATA lt_partners TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.

  SELECT parvw, kunnr
    FROM vbpa
    WHERE vbeln = @lv_contract
      AND kunnr <> ''
    INTO TABLE @DATA(lt_vbpa).

  lt_partners = VALUE #(
    FOR p IN lt_vbpa
    ( partn_role = p-parvw
      partn_numb = p-kunnr ) ).

  IF NOT line_exists( lt_partners[ partn_role = 'AG' ] ).
    APPEND VALUE bapipartnr( partn_role = 'AG' partn_numb = ls_vbak-kunnr ) TO lt_partners.
  ENDIF.

  "------------------------------------------------------------
  " 6) Build BAPI structures for CREATEFROMDATA1
  "------------------------------------------------------------
  DATA ls_head   TYPE bapisdhead1.
  DATA ls_headx  TYPE bapisdhead1x.

  CLEAR: ls_head, ls_headx.

  ls_head-doc_type   = gc_auart.
  ls_head-sales_org  = ls_vbak-vkorg.
  ls_head-distr_chan = ls_vbak-vtweg.
  ls_head-division   = ls_vbak-spart.
  ls_head-req_date_h = ls_input-deliverydate.
  ls_head-purch_no_c = ls_input-customerref.

  "Reference at header level (helps copy control in many systems)
  ls_head-ref_doc    = lv_contract.
  ls_head-ref_doc_ca = 'G'.

  ls_headx-updateflag = 'I'.
  ls_headx-doc_type   = 'X'.
  ls_headx-sales_org  = 'X'.
  ls_headx-distr_chan = 'X'.
  ls_headx-division   = 'X'.
  ls_headx-req_date_h = 'X'.
  ls_headx-purch_no_c = 'X'.
  ls_headx-ref_doc    = 'X'.
  ls_headx-ref_doc_ca = 'X'.

  DATA lt_items   TYPE STANDARD TABLE OF bapisditem  WITH DEFAULT KEY.
  DATA lt_itemsx  TYPE STANDARD TABLE OF bapisditemx WITH DEFAULT KEY.
  DATA lt_sched   TYPE STANDARD TABLE OF bapischedule  WITH DEFAULT KEY.
  DATA lt_schedx  TYPE STANDARD TABLE OF bapischedulx  WITH DEFAULT KEY.
  DATA lt_cond    TYPE STANDARD TABLE OF bapicondition WITH DEFAULT KEY.
  DATA lt_return  TYPE bapiret2_tab.

  "Optional: force German during SD text reads (your system has TSPAT only in DE)
  DATA(lv_old_langu) = sy-langu.
  SET LOCALE LANGUAGE 'D'.

  LOOP AT lt_agg INTO DATA(ls_agg).

    READ TABLE lt_ctr_items INTO ls_ctr WITH KEY posnr = ls_agg-posnr.
    IF sy-subrc <> 0.
      CONTINUE.
    ENDIF.

    "---------------- ITEM ----------------
    DATA ls_item  TYPE bapisditem.
    DATA ls_itemx TYPE bapisditemx.
    CLEAR: ls_item, ls_itemx.

    ls_item-itm_number = ls_ctr-posnr.
    ls_item-material   = ls_ctr-matnr.
    ls_item-plant      = ls_ctr-werks.
    ls_item-item_categ = ls_ctr-pstyv.

    "Zielmenge (Target qty)
    ls_item-target_qty = ls_agg-qty.
    ls_item-target_qu  = ls_ctr-vrkme.

    "Reference to contract item
    ls_item-ref_doc    = lv_contract.
    ls_item-ref_doc_it = ls_ctr-posnr.
    ls_item-ref_doc_ca = 'G'.

    APPEND ls_item TO lt_items.

    ls_itemx-itm_number = ls_ctr-posnr.
    ls_itemx-updateflag = 'I'.
    ls_itemx-material   = 'X'.
    ls_itemx-plant      = 'X'.
    ls_itemx-item_categ = 'X'.
    ls_itemx-target_qty = 'X'.
    ls_itemx-target_qu  = 'X'.
    ls_itemx-ref_doc    = 'X'.
    ls_itemx-ref_doc_it = 'X'.
    ls_itemx-ref_doc_ca = 'X'.

    APPEND ls_itemx TO lt_itemsx.

    "---------------- SCHEDULE (helps qty completeness) ----------------
    DATA ls_sc  TYPE bapischedule.
    DATA ls_scx TYPE bapischedulx.
    CLEAR: ls_sc, ls_scx.

    ls_sc-itm_number = ls_ctr-posnr.
    ls_sc-sched_line = '0001'.
    ls_sc-req_qty    = ls_agg-qty.
    ls_sc-req_date   = ls_input-deliverydate.
    APPEND ls_sc TO lt_sched.

    ls_scx-itm_number = ls_ctr-posnr.
    ls_scx-sched_line = '0001'.
    ls_scx-updateflag = 'I'.
    ls_scx-req_qty    = 'X'.
    ls_scx-req_date   = 'X'.
    APPEND ls_scx TO lt_schedx.

    "---------------- CONDITIONS (Net price/value completeness) ----------------
    "Pick a condition type from the contract pricing if possible; otherwise PR00
    DATA(lv_cond_type) TYPE kschl VALUE 'PR00'.

    IF ls_vbak-knumv IS NOT INITIAL.
      SELECT kschl
        FROM prcd_elements
        WHERE knumv = @ls_vbak-knumv
          AND kposn = @ls_ctr-posnr
          AND kschl <> ''
          AND kinak = ''
          AND kstat = ''
        INTO TABLE @DATA(lt_prcd_kschl).

      IF line_exists( lt_prcd_kschl[ kschl = 'PR00' ] ).
        lv_cond_type = 'PR00'.
      ELSEIF line_exists( lt_prcd_kschl[ kschl = 'PB00' ] ).
        lv_cond_type = 'PB00'.
      ELSEIF lines( lt_prcd_kschl ) > 0.
        READ TABLE lt_prcd_kschl INTO DATA(ls_first_kschl) INDEX 1.
        lv_cond_type = ls_first_kschl-kschl.
      ENDIF.
    ENDIF.

    "Contract net price
    IF ls_ctr-netpr IS NOT INITIAL.
      DATA ls_c TYPE bapicondition.
      CLEAR ls_c.

      ls_c-itm_number = ls_ctr-posnr.
      ls_c-cond_st_no = '010'.
      ls_c-cond_count = '01'.
      ls_c-cond_type  = lv_cond_type.
      ls_c-cond_value = ls_ctr-netpr.
      ls_c-currency   = ls_vbak-waerk.
      ls_c-cond_p_unt = COND #( WHEN ls_ctr-kpein IS INITIAL THEN 1 ELSE ls_ctr-kpein ).
      ls_c-cond_unit  = COND #( WHEN ls_ctr-kmein IS INITIAL THEN ls_ctr-vrkme ELSE ls_ctr-kmein ).

      APPEND ls_c TO lt_cond.
    ELSE.
      "If contract has no net price, SD will still be incomplete -> tell user clearly
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |No NETPR in contract item { ls_ctr-posnr } -> cannot fill Net price/value|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDIF.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL.
    SET LOCALE LANGUAGE lv_old_langu.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "------------------------------------------------------------
  " 7) Create via BAPI_SALESDOCU_CREATEFROMDATA1
  "------------------------------------------------------------
  DATA lv_new_doc TYPE bapivbeln-vbeln.

  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA1'
    EXPORTING
      sales_header_in   = ls_head
      sales_header_inx  = ls_headx
      int_number_assignment = space
    IMPORTING
      salesdocument_ex  = lv_new_doc
    TABLES
      return            = lt_return
      sales_items_in    = lt_items
      sales_items_inx   = lt_itemsx
      sales_partners    = lt_partners
      sales_schedules_in  = lt_sched
      sales_schedules_inx = lt_schedx
      sales_conditions_in = lt_cond.

  SET LOCALE LANGUAGE lv_old_langu.

  "------------------------------------------------------------
  " 8) Handle BAPI return
  "------------------------------------------------------------
  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ) OR lv_new_doc IS INITIAL.

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

  "Success
  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_doc
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Created successfully' ) )
    ) )
  ).

ENDMETHOD.
