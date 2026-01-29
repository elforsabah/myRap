METHOD createlanf.

    DATA:
      ls_header_in    TYPE bapisdhead,
      lt_items_in     TYPE bapisditm_tt,
      lt_items_inx    TYPE bapisditmx_tt, " Kept to satisfy map method signature
      lt_partners     TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
      lt_return       TYPE bapiret2_tab,
      ls_return1      TYPE bapireturn1,
      lv_vbeln        TYPE vbeln_va,
      ls_contract_hdr TYPE vbak,
      lv_vbtyp        TYPE tvak-vbtyp,
      lv_busobj       TYPE swo_objtyp.

    " 1. Get Input Key
    READ TABLE keys INTO DATA(ls_key) INDEX 1.
    DATA(ls_input) = ls_key-%param.

    " 2. Validate Contract Existence
    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.

    SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
    IF sy-subrc <> 0.
      APPEND VALUE #(
        %msg = new_message(
          id = '00' number = '001'
          v1 = |Contract { lv_contract } not found|
          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    " 3. Determine Business Object (for BAPI)
    DATA(lv_auart) = 'ZLRA'. " Adjust if needed

    SELECT SINGLE vbtyp FROM tvak INTO @lv_vbtyp WHERE auart = @lv_auart.
    IF sy-subrc = 0 AND lv_vbtyp IS NOT INITIAL.
      CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
        EXPORTING i_document_type   = lv_vbtyp
        IMPORTING e_business_object = lv_busobj.
    ENDIF.
    IF lv_busobj IS INITIAL.
      lv_busobj = 'BUS2032'. " Fallback to Standard Order
    ENDIF.

    " 4. Fill Header Data
    CLEAR ls_header_in.
    ls_header_in-doc_type   = lv_auart.
    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
    ls_header_in-division   = ls_contract_hdr-spart.
    
    " Reference to Contract
    ls_header_in-ref_doc    = lv_contract.
    ls_header_in-ref_doc_ca = 'G'. 
    
    " Dates and References
    ls_header_in-req_date_h = ls_input-deliverydate.
    ls_header_in-purch_no_c = ls_input-customerref.

    " 5. Fill Partners (Copy from Contract)
    SELECT * FROM vbpa
      WHERE vbeln = @lv_contract
      INTO TABLE @DATA(lt_contract_partners).

    lt_partners = CORRESPONDING #( lt_contract_partners
      MAPPING partn_role = parvw
              partn_numb = kunnr ).

    " 6. Fill Items (Map Input Materials to Contract Items)
    CLEAR: lt_items_in, lt_items_inx.
    map_positions_to_contract(
      EXPORTING
        iv_contract_vbeln = ls_input-ContractVbeln
        it_positions      = CORRESPONDING #( ls_input-_positions )
      CHANGING
        ct_order_items    = lt_items_in
        ct_order_itemsx   = lt_items_inx
        ct_return         = lt_return ).

    " Check for mapping errors before calling BAPI
    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
      LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<m>) WHERE type CA 'EA'.
        APPEND VALUE #(
          %msg = new_message(
            id = <m>-id number = <m>-number
            v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
            severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      ENDLOOP.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    " 7. Call BAPI
    " Note: BAPI_SALESDOCU_CREATEFROMDATA does NOT support EXTENSIONIN.
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

    " 8. Handle BAPI Return
    " Convert BAPIRETURN1 to BAPIRET2 format for consistent error handling
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

    IF ls_return1-type CA 'EAX'.
      " Error occurred
      CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.

      LOOP AT lt_return ASSIGNING <m> WHERE type CA 'EAX'.
        APPEND VALUE #(
          %msg = new_message(
            id = <m>-id number = <m>-number
            v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
            severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      ENDLOOP.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    " Success - Commit
    CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.

    " 9. Return Result
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
