METHOD createlanf.

    DATA:
      " NOTE: Type changed from BAPISDHEAD to BAPISDHD1 for DAT2 BAPI
      ls_header_in    TYPE bapisdhd1, 
      ls_header_inx   TYPE bapisdhd1x,
      
      " These types now MATCH the BAPI perfectly (No conversion needed)
      lt_items_in     TYPE bapisditm_tt,
      lt_items_inx    TYPE bapisditmx_tt,
      lt_partners     TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
      
      lt_return       TYPE bapiret2_tab,
      lv_vbeln        TYPE vbeln_va,
      ls_contract_hdr TYPE vbak,
      lv_vbtyp        TYPE tvak-vbtyp,
      lv_busobj       TYPE swo_objtyp.

    " 1. Get Input
    READ TABLE keys INTO DATA(ls_key) INDEX 1.
    DATA(ls_input) = ls_key-%param.

    " 2. Validate Contract
    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.
    SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Contract not found| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    " 3. Header Mapping (To BAPISDHD1)
    ls_header_in-doc_type   = 'ZLRA'. 
    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
    ls_header_in-division   = ls_contract_hdr-spart.
    ls_header_in-ref_doc    = lv_contract.
    ls_header_in-ref_doc_ca = 'G'.
    ls_header_in-req_date_h = ls_input-deliverydate.
    ls_header_in-purch_no_c = ls_input-customerref.
    
    " Header X-Structure (Required by DAT2 BAPI for some fields)
    ls_header_inx-doc_type   = 'X'.
    ls_header_inx-sales_org  = 'X'.
    ls_header_inx-distr_chan = 'X'.
    ls_header_inx-division   = 'X'.
    ls_header_inx-ref_doc    = 'X'.
    ls_header_inx-ref_doc_ca = 'X'.
    ls_header_inx-req_date_h = 'X'.
    ls_header_inx-purch_no_c = 'X'.
    ls_header_inx-updateflag = 'I'. " Insert

    " 4. Partners
    SELECT * FROM vbpa WHERE vbeln = @lv_contract INTO TABLE @DATA(lt_contract_partners).
    lt_partners = CORRESPONDING #( lt_contract_partners MAPPING partn_role = parvw partn_numb = kunnr ).

    " 5. Items (Direct Mapping - No manual loop needed!)
    " The DAT2 BAPI uses BAPISDITM, which matches your helper method's output.
    map_positions_to_contract(
      EXPORTING
        iv_contract_vbeln = ls_input-ContractVbeln
        it_positions      = CORRESPONDING #( ls_input-_positions )
      CHANGING
        ct_order_items    = lt_items_in
        ct_order_itemsx   = lt_items_inx
        ct_return         = lt_return ).

    IF line_exists( lt_return[ type = 'E' ] ).
       " ... (Error handling logic same as before) ...
       RETURN.
    ENDIF.

    " 6. Call the Modern BAPI
    CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
      EXPORTING
        order_header_in      = ls_header_in
        order_header_inx     = ls_header_inx
        " logic_switch       = ... (Optional: useful for pricing checks)
      IMPORTING
        salesdocument        = lv_vbeln
      TABLES
        return               = lt_return
        order_items_in       = lt_items_in
        order_items_inx      = lt_items_inx
        order_partners       = lt_partners.
        " extensionin        = lt_extensionin (Fully supported here if you need it!)

    " 7. Handle Return
    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
       CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
       " ... Map messages to reported ...
    ELSE.
       CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.
       " ... Map success to result ...
    ENDIF.

  ENDMETHOD.
