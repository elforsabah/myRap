METHOD createlanf.

  CONSTANTS gc_auart TYPE auart VALUE 'ZLRA'.   "Debit Memo Request type in your system

  "Helper macro: set component only if it exists (avoids syntax errors)
  DEFINE set_comp.
    ASSIGN COMPONENT &1 OF STRUCTURE &2 TO FIELD-SYMBOL(<fs>).
    IF sy-subrc = 0.
      <fs> = &3.
    ENDIF.
  END-OF-DEFINITION.

  "-----------------------------
  " 1) Input
  "-----------------------------
  DATA ls_key LIKE LINE OF keys.
  READ TABLE keys INTO ls_key INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA lv_contract TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  "-----------------------------
  " 2) Read contract header
  "-----------------------------
  DATA ls_vbak TYPE vbak.
  SELECT SINGLE *
    FROM vbak
    WHERE vbeln = @lv_contract
    INTO @ls_vbak.

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
  " 3) Read contract items incl. pricing fields
  "-----------------------------
  TYPES: BEGIN OF ty_ctr,
           posnr TYPE posnr_va,
           matnr TYPE matnr,
           werks TYPE werks_d,
           vrkme TYPE vrkme,
           netpr TYPE netpr,
           netwr TYPE netwr,
           kpein TYPE kpein,
           kmein TYPE kmein,
         END OF ty_ctr.

  DATA lt_ctr TYPE STANDARD TABLE OF ty_ctr WITH EMPTY KEY.

  SELECT posnr, matnr, werks, vrkme, netpr, netwr, kpein, kmein
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @lt_ctr.

  IF lt_ctr IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |No items found in contract { lv_contract }|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 4) Aggregate input positions by contract POSNR (prevents duplicates)
  "-----------------------------
  TYPES: BEGIN OF ty_agg,
           posnr TYPE posnr_va,
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

    READ TABLE lt_ctr INTO DATA(ls_ctr) WITH KEY matnr = lv_matnr.
    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Material { ls_pos-matnr } not found in contract { lv_contract }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    "Unit must match contract (no conversion here)
    IF ls_pos-meins IS NOT INITIAL AND ls_ctr-vrkme IS NOT INITIAL AND ls_pos-meins <> ls_ctr-vrkme.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Unit mismatch for { ls_pos-matnr }: input { ls_pos-meins } <> contract { ls_ctr-vrkme }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    ASSIGN lt_agg[ posnr = ls_ctr-posnr ] TO <agg>.
    IF sy-subrc = 0.
      <agg>-qty = <agg>-qty + ls_pos-menge.
    ELSE.
      INSERT VALUE ty_agg( posnr = ls_ctr-posnr qty = ls_pos-menge ) INTO TABLE lt_agg.
    ENDIF.

  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL OR lt_agg IS INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 5) Determine BOR object for doc category (VBTYP of ZLRA)
  "-----------------------------
  DATA lv_vbtyp TYPE tvak-vbtyp.
  SELECT SINGLE vbtyp FROM tvak INTO @lv_vbtyp WHERE auart = @gc_auart.

  DATA lv_busobj TYPE swo_objtyp.
  CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
    EXPORTING
      i_document_type   = lv_vbtyp
    IMPORTING
      e_business_object = lv_busobj.

  IF lv_busobj IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |Cannot determine BOR object for AUART { gc_auart } (VBTYP { lv_vbtyp })|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 6) Partners from contract
  "-----------------------------
  DATA lt_partners TYPE STANDARD TABLE OF bapipartnr WITH DEFAULT KEY.

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
  " 7) Prepare header
  "-----------------------------
  DATA ls_head TYPE bapisdhead.
  CLEAR ls_head.

  ls_head-doc_type   = gc_auart.
  ls_head-sales_org  = ls_vbak-vkorg.
  ls_head-distr_chan = ls_vbak-vtweg.
  ls_head-division   = ls_vbak-spart.

  ls_head-ref_doc    = lv_contract.
  ls_head-ref_doc_ca = 'G'.

  ls_head-req_date_h = ls_input-deliverydate.
  ls_head-purch_no_c = ls_input-customerref.

  "Force German language (because TSPAT has only German texts)
  set_comp 'LANGU' ls_head 'D'.

  "-----------------------------
  " 8) Prepare items with qty + net price + net value (CONCRETE FIX)
  "-----------------------------
  DATA lt_items TYPE STANDARD TABLE OF bapiitemin WITH DEFAULT KEY.
  CLEAR lt_items.

  LOOP AT lt_agg INTO DATA(ls_agg).

    READ TABLE lt_ctr INTO ls_ctr WITH KEY posnr = ls_agg-posnr.
    IF sy-subrc <> 0.
      CONTINUE.
    ENDIF.

    DATA ls_item TYPE bapiitemin.
    CLEAR ls_item.

    ls_item-itm_number = ls_ctr-posnr.
    ls_item-material   = ls_ctr-matnr.

    "---- Quantity (Zielmenge)
    set_comp 'TARGET_QTY' ls_item ls_agg-qty.
    set_comp 'REQ_QTY'    ls_item ls_agg-qty.      "fallback name in some releases
    set_comp 'TARGET_QU'  ls_item ls_ctr-vrkme.
    set_comp 'SALES_UNIT' ls_item ls_ctr-vrkme.    "fallback name

    "---- Reference
    set_comp 'REF_DOC'    ls_item lv_contract.
    set_comp 'REF_DOC_IT' ls_item ls_ctr-posnr.
    set_comp 'REF_DOC_CA' ls_item 'G'.

    "---- Plant (often required)
    set_comp 'PLANT'      ls_item ls_ctr-werks.

    "---- Net price/value (read from contract item)
    DATA lv_price TYPE netpr.
    DATA lv_pu    TYPE kpein.
    lv_price = ls_ctr-netpr.
    lv_pu    = ls_ctr-kpein.
    IF lv_pu IS INITIAL.
      lv_pu = 1.
    ENDIF.

    "net value = net price * qty / pricing unit
    DATA(lv_val) TYPE decfloat34.
    lv_val = CONV decfloat34( lv_price ) * CONV decfloat34( ls_agg-qty ) / CONV decfloat34( lv_pu ).

    DATA lv_netwr TYPE netwr.
    lv_netwr = lv_val.

    "Try multiple possible component names
    set_comp 'NET_PRICE'  ls_item lv_price.
    set_comp 'NETPR'      ls_item lv_price.
    set_comp 'NET_VALUE'  ls_item lv_netwr.
    set_comp 'NETWR'      ls_item lv_netwr.

    "Currency (if field exists in structure)
    set_comp 'CURRENCY'   ls_item ls_vbak-waerk.
    set_comp 'CURRENCY_ISO' ls_item ls_vbak-waerk.

    "Pricing unit fields (if exist)
    set_comp 'COND_P_UNT' ls_item lv_pu.
    set_comp 'PRICE_UNIT' ls_item lv_pu.
    set_comp 'COND_UNIT'  ls_item ls_ctr-kmein.

    APPEND ls_item TO lt_items.

  ENDLOOP.

  IF lt_items IS INITIAL.
    APPEND VALUE #( %msg = new_message(
      id       = '00'
      number   = '001'
      v1       = |No items prepared for creation|
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "-----------------------------
  " 9) Call BAPI (force German locale during call)
  "-----------------------------
  DATA lv_old_langu TYPE sylangu VALUE sy-langu.
  DATA lv_new_doc   TYPE vbeln_va.
  DATA ls_ret1      TYPE bapireturn1.

  TRY.
      SET LOCALE LANGUAGE 'D'.

      CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
        EXPORTING
          order_header_in = ls_head
          business_object = lv_busobj
          without_commit  = abap_true
        IMPORTING
          salesdocument   = lv_new_doc
          return          = ls_ret1
        TABLES
          order_items_in  = lt_items
          order_partners  = lt_partners.

    FINALLY.
      SET LOCALE LANGUAGE lv_old_langu.
  ENDTRY.

  "-----------------------------
  " 10) Handle result
  "-----------------------------
  IF lv_new_doc IS INITIAL OR ls_ret1-type CA 'EAX'.

    "Return BAPI message
    APPEND VALUE #( %msg = new_message(
      id       = ls_ret1-id
      number   = ls_ret1-number
      v1       = ls_ret1-message_v1
      v2       = ls_ret1-message_v2
      v3       = ls_ret1-message_v3
      v4       = ls_ret1-message_v4
      severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.

    "Extra helpful hint when still incomplete
    IF ls_ret1-id = 'SLS_LORD' AND ls_ret1-number = '009'.
      APPEND VALUE #( %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Still incomplete: check whether net price can be set manually|
        v2       = |If contract NETPR is initial or pricing forbids manual price, you must maintain pricing conditions.|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDIF.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Success
  result = VALUE #(
    ( %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_new_doc
        _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success' ) )
    ) )
  ).

ENDMETHOD.
