METHOD createlanf.
  DATA:
    ls_header_in TYPE bapisdhd1,
    ls_header_inx TYPE bapisdhd1x,
    lt_items_in TYPE bapisditm_tt,
    lt_items_inx TYPE bapisditmx_tt,
    lt_partners TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
    lt_return TYPE bapiret2_tab,
    lv_vbeln TYPE vbeln_va,
    ls_contract_hdr TYPE vbak,
    ls_item_in TYPE bapisditm,
    ls_item_inx TYPE bapisditmx.

  " 1. Get Input Key
  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  " 2. Validate Contract
  DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.
  SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
  IF sy-subrc <> 0.
    APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Contract { lv_contract } not found| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  " 3. Header Mapping
  ls_header_in-doc_type = 'ZLRA'. " Check if this Order Type is correct
  ls_header_in-sales_org = ls_contract_hdr-vkorg.
  ls_header_in-distr_chan = ls_contract_hdr-vtweg.
  ls_header_in-division = ls_contract_hdr-spart.
  ls_header_in-ref_doc = lv_contract.
  ls_header_in-ref_doc_ca = 'G'. " 'G' for contract
  ls_header_in-req_date_h = ls_input-deliverydate.
  ls_header_in-purch_no_c = ls_input-customerref.

  " Header Checkbox (INX)
  ls_header_inx-doc_type = 'X'.
  ls_header_inx-sales_org = 'X'.
  ls_header_inx-distr_chan = 'X'.
  ls_header_inx-division = 'X'.
  ls_header_inx-ref_doc = 'X'.
  ls_header_inx-ref_doc_ca = 'X'.
  ls_header_inx-req_date_h = 'X'.
  ls_header_inx-purch_no_c = 'X'.
  ls_header_inx-updateflag = 'I'. " Insert

  " 4. Partners (Copy from Contract)
  SELECT * FROM vbpa WHERE vbeln = @lv_contract INTO TABLE @DATA(lt_contract_partners).
  lt_partners = CORRESPONDING #( lt_contract_partners MAPPING partn_role = parvw partn_numb = kunnr ).

  " 5. Item Mapping (INLINE - To Fix Material Mapping Error)
  " Pre-fetch contract items to verify materials
  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @lv_contract
    INTO TABLE @DATA(lt_contract_pos).

  LOOP AT ls_input-_positions INTO DATA(ls_pos_input).
    " Normalize Input Material
    DATA(lv_matnr_long) = |{ ls_pos_input-matnr ALPHA = IN }|.

    " Find Position in Contract
    READ TABLE lt_contract_pos INTO DATA(ls_contr_item) WITH KEY matnr = lv_matnr_long.
    IF sy-subrc <> 0.
      " Fallback: Try without ALPHA if needed
      READ TABLE lt_contract_pos INTO ls_contr_item WITH KEY matnr = ls_pos_input-matnr.
    ENDIF.

    IF ls_contr_item-posnr IS INITIAL.
      APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Material { ls_pos_input-matnr } not found in Contract| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      CONTINUE.
    ENDIF.

    CLEAR: ls_item_in, ls_item_inx.

    " --- DATA (Use only MATERIAL_LONG to avoid mapping ambiguity) ---
    ls_item_in-itm_number = ls_contr_item-posnr.
    CLEAR ls_item_in-material.  " Explicitly clear short field
    ls_item_in-material_long = lv_matnr_long.  " Use long field
    ls_item_in-target_qty = ls_pos_input-menge.
    ls_item_in-target_qu = ls_pos_input-meins.
    ls_item_in-ref_doc = lv_contract.
    ls_item_in-ref_doc_it = ls_contr_item-posnr.
    ls_item_in-ref_doc_ca = 'G'. " 'G' for contract

    " --- CHECKBOX (INX) (Mark only long material) ---
    ls_item_inx-itm_number = ls_contr_item-posnr.
    ls_item_inx-updateflag = 'I'.
    CLEAR ls_item_inx-material.  " Blank (not ' ')
    ls_item_inx-material_long = 'X'.
    ls_item_inx-target_qty = 'X'.
    ls_item_inx-target_qu = 'X'.
    ls_item_inx-ref_doc = 'X'.
    ls_item_inx-ref_doc_it = 'X'.
    ls_item_inx-ref_doc_ca = 'X'.

    APPEND ls_item_in TO lt_items_in.
    APPEND ls_item_inx TO lt_items_inx.
  ENDLOOP.

  IF reported-lanfroot IS NOT INITIAL.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  " 6. Call BAPI (Safe for RAP - No Commit/Rollback inside)
  CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
    EXPORTING
      order_header_in = ls_header_in
      order_header_inx = ls_header_inx
    IMPORTING
      salesdocument = lv_vbeln
    TABLES
      return = lt_return
      order_items_in = lt_items_in
      order_items_inx = lt_items_inx
      order_partners = lt_partners.

  " 7. Handle Return
  IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
    LOOP AT lt_return INTO DATA(ls_err) WHERE type CA 'EA'.
      APPEND VALUE #(
        %msg = new_message(
          id = ls_err-id number = ls_err-number
          v1 = ls_err-message_v1 v2 = ls_err-message_v2
          v3 = ls_err-message_v3 v4 = ls_err-message_v4
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.
    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
  ELSE.
    " Success! RAP framework handles commit after method exit.
    result = VALUE #(
      ( %param = VALUE #(
          techkey = 'X'
          vbeln = lv_vbeln
          _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success' ) )
        ) ) ).
  ENDIF.
ENDMETHOD.
