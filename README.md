METHOD createlanf.

    DATA:
      ls_header_in      TYPE bapisdhead,
      
      " -- WRONG TYPES (Used by your helper method) --
      lt_items_helper   TYPE bapisditm_tt,
      lt_items_inx      TYPE bapisditmx_tt, 
      lt_partners_temp  TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,

      " -- CORRECT TYPES (Required by BAPI_SALESDOCU_CREATEFROMDATA) --
      lt_items_bapi     TYPE STANDARD TABLE OF bapiitemin,
      lt_partners_bapi  TYPE STANDARD TABLE OF bapipartnr,
      
      lt_return         TYPE bapiret2_tab,
      ls_return1        TYPE bapireturn1,
      lv_vbeln          TYPE vbeln_va,
      ls_contract_hdr   TYPE vbak,
      lv_vbtyp          TYPE tvak-vbtyp,
      lv_busobj         TYPE swo_objtyp.

    " 1. Get Input Key
    READ TABLE keys INTO DATA(ls_key) INDEX 1.
    DATA(ls_input) = ls_key-%param.

    " 2. Validate Contract
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

    " 3. Determine Business Object
    DATA(lv_auart) = 'ZLRA'. 

    SELECT SINGLE vbtyp FROM tvak INTO @lv_vbtyp WHERE auart = @lv_auart.
    IF sy-subrc = 0 AND lv_vbtyp IS NOT INITIAL.
      CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
        EXPORTING i_document_type   = lv_vbtyp
        IMPORTING e_business_object = lv_busobj.
    ENDIF.
    IF lv_busobj IS INITIAL.
      lv_busobj = 'BUS2032'. 
    ENDIF.

    " 4. Fill Header 
    CLEAR ls_header_in.
    ls_header_in-doc_type   = lv_auart.
    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
    ls_header_in-division   = ls_contract_hdr-spart.
    ls_header_in-ref_doc    = lv_contract.
    ls_header_in-ref_doc_ca = 'G'. 
    ls_header_in-req_date_h = ls_input-deliverydate.
    ls_header_in-purch_no_c = ls_input-customerref.

    " 5. Fill Partners (Intermediate Step)
    SELECT * FROM vbpa
      WHERE vbeln = @lv_contract
      INTO TABLE @DATA(lt_contract_partners).

    lt_partners_temp = CORRESPONDING #( lt_contract_partners
      MAPPING partn_role = parvw
              partn_numb = kunnr ).

    " >>> FIX: Convert to correct BAPI structure (BAPIPARTNR)
    lt_partners_bapi = CORRESPONDING #( lt_partners_temp ).

    " 6. Fill Items (Intermediate Step)
    CLEAR: lt_items_helper, lt_items_inx.
    
    " Your helper method returns BAPISDITM (which causes the crash)
    map_positions_to_contract(
      EXPORTING
        iv_contract_vbeln = ls_input-ContractVbeln
        it_positions      = CORRESPONDING #( ls_input-_positions )
      CHANGING
        ct_order_items    = lt_items_helper
        ct_order_itemsx   = lt_items_inx
        ct_return         = lt_return ).

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

    " >>> FIX: Convert Items to correct BAPI structure (BAPIITEMIN)
    LOOP AT lt_items_helper ASSIGNING FIELD-SYMBOL(<helper_item>).
      APPEND VALUE bapiitemin(
        itm_number = <helper_item>-itm_number
        material   = <helper_item>-material
        target_qty = <helper_item>-target_qty
        target_qu  = <helper_item>-target_qu
        ref_doc    = <helper_item>-ref_doc
        ref_doc_it = <helper_item>-ref_doc_it
        ref_doc_ca = <helper_item>-ref_doc_ca
      ) TO lt_items_bapi.
    ENDLOOP.

    " 7. Call BAPI with CORRECTED tables
    CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
      EXPORTING
        order_header_in = ls_header_in
        business_object = lv_busobj
        without_commit  = abap_true
      IMPORTING
        salesdocument   = lv_vbeln
        return          = ls_return1
      TABLES
        order_items_in  = lt_items_bapi    " Type BAPIITEMIN
        order_partners  = lt_partners_bapi. " Type BAPIPARTNR

    " 8. Handle BAPI Return
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
