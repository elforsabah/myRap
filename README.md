METHOD createlanf.

  DATA:
    ls_header_in    TYPE bapisdhd1,
    ls_header_inx   TYPE bapisdhd1x,
    lt_items_in     TYPE bapisditm_tt,
    lt_items_inx    TYPE bapisditmx_tt,
    lt_partners     TYPE bapiparnr_t,
    lt_return       TYPE bapiret2_tab,
    lv_vbeln        TYPE vbeln_va,
    ls_contract_hdr TYPE vbak.

  DATA: lt_extensionin TYPE STANDARD TABLE OF bapiparex WITH DEFAULT KEY,
        ls_extensionin TYPE bapiparex,
        ls_bape_vbak   TYPE bape_vbak,
        ls_bape_vbakx  TYPE bape_vbakx.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  " Normalize VBELN (contract)
  DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.

  SELECT SINGLE * FROM vbak
    WHERE vbeln = @lv_contract
    INTO @ls_contract_hdr.
  IF sy-subrc <> 0.
    APPEND VALUE #(
      %msg = new_message(
        id = '00' number = '001'
        v1 = |Contract { lv_contract } not found|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  " Header (NO ls_header_in-name!)
  CLEAR ls_header_in.
  ls_header_in-doc_type   = 'DR'.
  ls_header_in-sales_org  = ls_contract_hdr-vkorg.
  ls_header_in-distr_chan = ls_contract_hdr-vtweg.
  ls_header_in-division   = ls_contract_hdr-spart.
  ls_header_in-ref_doc    = lv_contract.
  ls_header_in-refdoc_cat = 'G'.
  ls_header_in-req_date_h = ls_input-deliverydate.
  ls_header_in-purch_no_c = ls_input-customerref.

  CLEAR ls_header_inx.
  ls_header_inx-updateflag = 'I'.
  ls_header_inx-doc_type   = 'X'.
  ls_header_inx-sales_org  = 'X'.
  ls_header_inx-distr_chan = 'X'.
  ls_header_inx-division   = 'X'.
  ls_header_inx-ref_doc    = 'X'.
  ls_header_inx-refdoc_cat = 'X'.
  ls_header_inx-req_date_h = 'X'.
  ls_header_inx-purch_no_c = 'X'.

  " Partners from contract
  SELECT * FROM vbpa
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_contract_partners).

  lt_partners = CORRESPONDING #( lt_contract_partners
    MAPPING partn_role = parvw
            partn_numb = kunnr ).

  " Map items (fix: NO inline DATA(...) inside parameter list)
  CLEAR: lt_items_in, lt_items_inx.
  map_positions_to_contract(
    EXPORTING
      iv_contract_vbeln = lv_contract
      it_positions      = CORRESPONDING #( ls_input-_positions )
    CHANGING
      ct_order_items    = lt_items_in
      ct_order_itemsx   = lt_items_inx
      ct_return         = lt_return ).

  " If mapping already produced errors -> stop
  IF line_exists( lt_return[ type = 'E' ] ).
    LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<m>) WHERE type = 'E'.
      APPEND VALUE #(
        %msg = new_message(
          id = <m>-id number = <m>-number
          v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  " --- Set Description (VBAK-KTEXT) via EXTENSIONIN ---
  CLEAR: ls_bape_vbak, ls_bape_vbakx, lt_extensionin.

  ls_bape_vbak-ktext  = ls_input-description.
  ls_bape_vbakx-ktext = abap_true.

  CLEAR ls_extensionin.
  ls_extensionin-structure  = 'BAPE_VBAK'.
  ls_extensionin-valuepart1 = ls_bape_vbak.
  APPEND ls_extensionin TO lt_extensionin.

  CLEAR ls_extensionin.
  ls_extensionin-structure  = 'BAPE_VBAKX'.
  ls_extensionin-valuepart1 = ls_bape_vbakx.
  APPEND ls_extensionin TO lt_extensionin.

  " Call BAPI
  CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
    EXPORTING
      order_header_in  = ls_header_in
      order_header_inx = ls_header_inx
    IMPORTING
      salesdocument    = lv_vbeln
    TABLES
      return           = lt_return
      order_items_in   = lt_items_in
      order_items_inx  = lt_items_inx
      order_partners   = lt_partners
      extensionin      = lt_extensionin.

  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.

    LOOP AT lt_return ASSIGNING <m> WHERE type CA 'EA'.
      APPEND VALUE #(
        %msg = new_message(
          id = <m>-id number = <m>-number
          v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.

  result = VALUE #(
    ( %param = VALUE #(
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
            msgv4   = r-message_v4 ) ) ) ) ).

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




