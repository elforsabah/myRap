METHOD createlanf.

  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.  "your target doc type

  "------------------------------------------------------------
  " Input
  "------------------------------------------------------------
  DATA ls_key LIKE LINE OF keys.
  READ TABLE keys INTO ls_key INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA lv_refdoc TYPE vbeln_va.
  lv_refdoc = |{ ls_input-contractvbeln ALPHA = IN }|.

  "------------------------------------------------------------
  " Validate reference doc exists + org data
  "------------------------------------------------------------
  DATA ls_ref_hdr TYPE vbak.
  SELECT SINGLE *
    FROM vbak
    WHERE vbeln = @lv_refdoc
    INTO @ls_ref_hdr.

  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |Reference document { lv_refdoc } not found|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "------------------------------------------------------------
  " Determine document category (TVAK-VBTYP) for the target AUART
  "------------------------------------------------------------
  DATA lv_vbtyp TYPE tvak-vbtyp.
  SELECT SINGLE vbtyp
    FROM tvak
    WHERE auart = @gc_auart
    INTO @lv_vbtyp.

  IF sy-subrc <> 0 OR lv_vbtyp IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |No TVAK entry / VBTYP found for AUART { gc_auart }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "------------------------------------------------------------
  " Read reference items for MATNR->POSNR mapping + plant
  "------------------------------------------------------------
  TYPES: BEGIN OF ty_ref_item,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           werks TYPE werks_d,
         END OF ty_ref_item.
  DATA lt_ref_items TYPE STANDARD TABLE OF ty_ref_item WITH EMPTY KEY.

  SELECT posnr, matnr, werks
    FROM vbap
    WHERE vbeln = @lv_refdoc
    INTO TABLE @lt_ref_items.

  "------------------------------------------------------------
  " Aggregate positions by reference POSNR (prevents duplicate ITM_NUMBER => M_/011)
  "------------------------------------------------------------
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
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

    READ TABLE lt_ref_items INTO DATA(ls_ref_it) WITH KEY matnr = lv_matnr.
    IF sy-subrc <> 0 OR ls_ref_it-posnr IS INITIAL.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Material { ls_pos-matnr } not found in reference { lv_refdoc }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    ASSIGN lt_agg[ posnr = ls_ref_it-posnr ] TO <agg>.
    IF sy-subrc = 0.
      IF <agg>-meins <> ls_pos-meins.
        APPEND VALUE #( %msg = new_message(
          id       = '00'
          number   = '001'
          v1       = |Same item { ls_ref_it-posnr } has different UoM ({ <agg>-meins } vs { ls_pos-meins })|
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
        CONTINUE.
      ENDIF.
      <agg>-qty = <agg>-qty + ls_pos-menge.
    ELSE.
      INSERT VALUE ty_agg(
        posnr = ls_ref_it-posnr
        matnr = lv_matnr
        werks = ls_ref_it-werks
        meins = ls_pos-meins
        qty   = ls_pos-menge ) INTO TABLE lt_agg.
    ENDIF.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL OR lt_agg IS INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "------------------------------------------------------------
  " Partners from reference doc
  "------------------------------------------------------------
  DATA lt_partners_dat2 TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY.
  DATA lt_partners_gen  TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.

  SELECT parvw, kunnr
    FROM vbpa
    WHERE vbeln = @lv_refdoc
      AND kunnr <> ''
      AND parvw IN ('AG','WE','RE','RG','PY')
    INTO TABLE @DATA(lt_vbpa).

  lt_partners_dat2 = VALUE #(
    FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).

  lt_partners_gen = VALUE #(
    FOR p IN lt_vbpa ( partn_role = p-parvw partn_numb = p-kunnr ) ).

  "============================================================
  " BRANCH A: VBTYP = 'C' => Sales Order API (BUS2032)
  "============================================================
  IF lv_vbtyp = 'C'.

    DATA: ls_head_in   TYPE bapisdhd1,
          ls_head_inx  TYPE bapisdhd1x,
          ls_logic     TYPE bapisdls,
          lt_items_in  TYPE bapisditm_tt,
          lt_items_inx TYPE bapisditmx_tt,
          lt_sch_in    TYPE STANDARD TABLE OF bapischdl  WITH DEFAULT KEY,
          lt_sch_inx   TYPE STANDARD TABLE OF bapischdlx WITH DEFAULT KEY,
          lt_return    TYPE bapiret2_tab,
          lv_new_vbeln TYPE vbeln_va.

    CLEAR: ls_head_in, ls_head_inx, ls_logic, lt_items_in, lt_items_inx, lt_sch_in, lt_sch_inx, lt_return, lv_new_vbeln.

    ls_head_in-doc_type    = gc_auart.
    ls_head_in-sales_org   = ls_ref_hdr-vkorg.
    ls_head_in-distr_chan  = ls_ref_hdr-vtweg.
    ls_head_in-division    = ls_ref_hdr-spart.
    ls_head_in-ref_doc     = lv_refdoc.
    ls_head_in-refdoc_cat  = 'G'. "contract reference in your scenario
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

    ls_logic-cond_handl = 'X'.

    LOOP AT lt_agg INTO DATA(ls_agg).

      DATA: ls_item_in  TYPE bapisditm,
            ls_item_inx TYPE bapisditmx,
            ls_sch1     TYPE bapischdl,
            ls_sch1x    TYPE bapischdlx.

      CLEAR: ls_item_in, ls_item_inx, ls_sch1, ls_sch1x.

      ls_item_in-itm_number = ls_agg-posnr.
      ls_item_in-material   = ls_agg-matnr.
      ls_item_in-target_qty = ls_agg-qty.
      ls_item_in-target_qu  = ls_agg-meins.
      ls_item_in-ref_doc    = lv_refdoc.
      ls_item_in-ref_doc_it = ls_agg-posnr.
      ls_item_in-ref_doc_ca = 'G'.
      IF ls_agg-werks IS NOT INITIAL.
        ls_item_in-plant = ls_agg-werks.
      ENDIF.
      APPEND ls_item_in TO lt_items_in.

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

      ls_sch1-itm_number = ls_agg-posnr.
      ls_sch1-sched_line = '0001'.
      ls_sch1-req_qty    = ls_agg-qty.
      ls_sch1-req_date   = ls_input-deliverydate.
      APPEND ls_sch1 TO lt_sch_in.

      ls_sch1x-itm_number = ls_agg-posnr.
      ls_sch1x-sched_line = '0001'.
      ls_sch1x-updateflag = 'I'.
      ls_sch1x-req_qty    = 'X'.
      ls_sch1x-req_date   = 'X'.
      APPEND ls_sch1x TO lt_sch_inx.

    ENDLOOP.

    TRY.
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
            order_schedules_in   = lt_sch_in
            order_schedules_inx  = lt_sch_inx
            order_partners       = lt_partners_dat2.
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

    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ) OR lv_new_vbeln IS INITIAL.
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
      IF lv_new_vbeln IS INITIAL.
        APPEND VALUE #( %msg = new_message(
          id       = '00'
          number   = '001'
          v1       = |No document created|
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      ENDIF.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    result = VALUE #(
      ( %param = VALUE #(
          techkey   = 'X'
          vbeln     = lv_new_vbeln
          _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (DAT2)' ) )
      ) )
    ).

    RETURN.
  ENDIF.

  "============================================================
  " BRANCH B: VBTYP <> 'C' (e.g. 'L') => Generic Sales Doc BAPI
  "============================================================
  DATA lv_busobj TYPE swo_objtyp.
  CLEAR lv_busobj.

  CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
    EXPORTING
      i_document_type   = lv_vbtyp
    IMPORTING
      e_business_object = lv_busobj.

  IF lv_busobj IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |Cannot determine BOR object for doc category { lv_vbtyp }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  DATA: ls_head_gen TYPE bapisdhead,
        lt_item_gen TYPE STANDARD TABLE OF bapiitemin WITH DEFAULT KEY,
        lt_ret2     TYPE bapiret2_tab,
        ls_ret1     TYPE bapireturn1,
        lv_new_doc  TYPE vbeln_va.

  CLEAR: ls_head_gen, lt_item_gen, lt_ret2, ls_ret1, lv_new_doc.

  ls_head_gen-doc_type   = gc_auart.
  ls_head_gen-sales_org  = ls_ref_hdr-vkorg.
  ls_head_gen-distr_chan = ls_ref_hdr-vtweg.
  ls_head_gen-division   = ls_ref_hdr-spart.

  ls_head_gen-ref_doc    = lv_refdoc.
  ls_head_gen-ref_doc_ca = 'G'.  "your reference is contract; adjust if not a contract
  ls_head_gen-req_date_h = ls_input-deliverydate.
  ls_head_gen-purch_no_c = ls_input-customerref.

  LOOP AT lt_agg INTO DATA(ls_agg2).
    APPEND VALUE bapiitemin(
      itm_number = ls_agg2-posnr
      material   = ls_agg2-matnr
      target_qty = ls_agg2-qty
      target_qu  = ls_agg2-meins
      ref_doc    = lv_refdoc
      ref_doc_it = ls_agg2-posnr
      ref_doc_ca = 'G'
      plant      = ls_agg2-werks ) TO lt_item_gen.
  ENDLOOP.

  TRY.
      CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
        EXPORTING
          order_header_in = ls_head_gen
          business_object = lv_busobj
          without_commit  = abap_true
        IMPORTING
          salesdocument   = lv_new_doc
          return          = ls_ret1
        TABLES
          order_items_in  = lt_item_gen
          order_partners  = lt_partners_gen.
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

  "Convert RETURN1 to RAP message
  IF ls_ret1-type CA 'EAX' OR lv_new_doc IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = ls_ret1-id
      number   = ls_ret1-number
      v1       = ls_ret1-message_v1
      v2       = ls_ret1-message_v2
      v3       = ls_ret1-message_v3
      v4       = ls_ret1-message_v4
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_doc
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success (GENERIC)' ) )
    ) )
  ).

ENDMETHOD.
