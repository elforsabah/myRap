METHOD createlanf.

  DATA:
    ls_header_in        TYPE bapisdh1,          "header for sales docu create (works in many systems)
    lt_items_in         TYPE STANDARD TABLE OF bapisditm WITH DEFAULT KEY,
    lt_partners         TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
    lt_return           TYPE bapiret2_tab,
    ls_return1          TYPE bapireturn1,
    lv_vbeln            TYPE vbeln_va,
    ls_contract_hdr     TYPE vbak,
    lv_busobj           TYPE nast-objtype,
    lv_vbtyp            TYPE vbak-vbtyp.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  "ALPHA input for VBELN (important!)
  DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.

  " Contract exists?
  SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
  IF sy-subrc <> 0.
    APPEND VALUE #(
      %msg = new_message(
        id       = '00'
        number   = '001'
        v1       = |Contract { lv_contract } not found|
        severity = if_abap_behv_message=>severity-error )
    ) TO reported-lanfroot.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  " -----------------------
  " Determine BUSINESS_OBJECT from AUART -> TVAK-VBTYP -> SD_OBJECT_TYPE_DETERMINE
  " -----------------------
  " IMPORTANT: set your LANF doc type here (DR is just an example)
  ls_header_in-doc_type   = 'DR'. " <-- if your LANF is ZLNF, put ZLNF here

  SELECT SINGLE vbtyp FROM tvak INTO @lv_vbtyp WHERE auart = @ls_header_in-doc_type.
  IF sy-subrc = 0 AND lv_vbtyp IS NOT INITIAL.
    CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
      EXPORTING
        i_document_type   = lv_vbtyp
      IMPORTING
        e_business_object = lv_busobj.
  ENDIF.

  IF lv_busobj IS INITIAL.
    lv_busobj = 'BUS2032'. "fallback (standard sales order)
  ENDIF.

  " -----------------------
  " Header
  " -----------------------
  ls_header_in-sales_org  = ls_contract_hdr-vkorg.
  ls_header_in-distr_chan = ls_contract_hdr-vtweg.
  ls_header_in-division   = ls_contract_hdr-spart.
  ls_header_in-ref_doc    = lv_contract.
  ls_header_in-refdoc_cat = 'G'.                 "Contract reference
  ls_header_in-req_date_h = ls_input-deliverydate.
  ls_header_in-purch_no_c = ls_input-customerref.
  ls_header_in-name       = ls_input-description.

  " -----------------------
  " Partners from contract
  " -----------------------
  SELECT * FROM vbpa
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_contract_partners).

  lt_partners = CORRESPONDING #(
    lt_contract_partners
    MAPPING partn_role = parvw
            partn_numb = kunnr ).

  " -----------------------
  " Map items (incl. mapping errors to lt_return)
  " -----------------------
  CLEAR lt_items_in.
  map_positions_to_contract(
    EXPORTING
      iv_contract_vbeln = lv_contract
      it_positions      = CORRESPONDING #( ls_input-_positions )
    CHANGING
      ct_order_items    = lt_items_in
      ct_order_itemsx   = DATA(lt_items_inx_dummy) "not used here, but keep signature
      ct_return         = lt_return ).

  " If mapping already produced errors -> stop
  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ) OR line_exists( lt_return[ type = 'X' ] ).
    LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<m1>) WHERE type CA 'EAX'.
      APPEND VALUE #(
        %msg = new_message(
          id       = <m1>-id
          number   = <m1>-number
          v1       = <m1>-message_v1
          v2       = <m1>-message_v2
          v3       = <m1>-message_v3
          v4       = <m1>-message_v4
          severity = if_abap_behv_message=>severity-error )
      ) TO reported-lanfroot.
    ENDLOOP.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  " -----------------------
  " Create Sales Document (generic, supports BUS2096 etc.)
  " -----------------------
  CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
    EXPORTING
      order_header_in = ls_header_in
      business_object = lv_busobj
      without_commit  = abap_true
    IMPORTING
      salesdocument   = lv_vbeln
      return          = ls_return1
    TABLES
      order_items_in  = lt_items_in
      order_partners  = lt_partners.

  " Convert RETURN1 -> RET2 and add to lt_return for unified mapping
  APPEND VALUE bapiret2(
    type       = ls_return1-type
    id         = ls_return1-id
    number     = ls_return1-number
    message    = ls_return1-message
    message_v1 = ls_return1-message_v1
    message_v2 = ls_return1-message_v2
    message_v3 = ls_return1-message_v3
    message_v4 = ls_return1-message_v4
  ) TO lt_return.

  " Error?
  IF ls_return1-type CA 'EAX'.
    CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.

    LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<ret>) WHERE type CA 'EAX'.
      APPEND VALUE #(
        %msg = new_message(
          id       = <ret>-id
          number   = <ret>-number
          v1       = <ret>-message_v1
          v2       = <ret>-message_v2
          v3       = <ret>-message_v3
          v4       = <ret>-message_v4
          severity = if_abap_behv_message=>severity-error )
      ) TO reported-lanfroot.
    ENDLOOP.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.

  " -----------------------
  " Result payload (VBELN + messages)
  " -----------------------
  result = VALUE #(
    (
      %param = VALUE #(
        techkey   = 'X'
        vbeln     = lv_vbeln
        _messages = VALUE #(
          FOR r IN lt_return (
            techkey = 'X'
            msgid   = r-id
            msgno   = r-number
            msgty   = r-type
            msgv1   = r-message_v1
            msgv2   = r-message_v2
            msgv3   = r-message_v3
            msgv4   = r-message_v4
          )
        )
      )
    )
  ).

ENDMETHOD.
