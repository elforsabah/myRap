METHOD createlanf.

    DATA:
      ls_header_in      TYPE bapisdhd1,
      ls_header_inx     TYPE bapisdhd1x,
      lt_items_in       TYPE bapisditm_tt,
      lt_items_inx      TYPE bapisditmx_tt,
      lt_partners       TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
      lt_return         TYPE bapiret2_tab,
      lv_vbeln          TYPE vbeln_va,
      ls_contract_hdr   TYPE vbak,
      lv_vbtyp          TYPE tvak-vbtyp,
      lv_busobj         TYPE swo_objtyp.

    " ... [Keep your Input Reading & Data Selection Logic same as before] ...
    READ TABLE keys INTO DATA(ls_key) INDEX 1.
    DATA(ls_input) = ls_key-%param.
    
    " ... [Keep your Validation & Mapping Logic] ...
    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.
    " ... (Select VBAK, Partners, Map Items etc.) ...
    
    map_positions_to_contract(
      EXPORTING
        iv_contract_vbeln = ls_input-ContractVbeln
        it_positions      = CORRESPONDING #( ls_input-_positions )
      CHANGING
        ct_order_items    = lt_items_in
        ct_order_itemsx   = lt_items_inx
        ct_return         = lt_return ).

    " Check Mapping Errors
    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
       " Map to REPORTED / FAILED
       LOOP AT lt_return INTO DATA(ls_ret_map) WHERE type CA 'EA'.
         APPEND VALUE #(
           %msg = new_message(
             id = ls_ret_map-id number = ls_ret_map-number
             v1 = ls_ret_map-message_v1 v2 = ls_ret_map-message_v2 
             v3 = ls_ret_map-message_v3 v4 = ls_ret_map-message_v4
             severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
       ENDLOOP.
       failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
       RETURN.
    ENDIF.

    " -------------------------------------------------------------
    " CALL BAPI (Without Commit/Rollback)
    " -------------------------------------------------------------
    CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
      EXPORTING
        order_header_in      = ls_header_in
        order_header_inx     = ls_header_inx
      IMPORTING
        salesdocument        = lv_vbeln
      TABLES
        return               = lt_return
        order_items_in       = lt_items_in
        order_items_inx      = lt_items_inx
        order_partners       = lt_partners.

    " -------------------------------------------------------------
    " HANDLE BAPI RESULT (RAP Style)
    " -------------------------------------------------------------
    
    " Check for Errors
    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
      
      " !!! CRITICAL CHANGE: DO NOT CALL BAPI_TRANSACTION_ROLLBACK !!!
      " Just report the failure. RAP will handle the rollback of the LUW.
      
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
      
      " !!! CRITICAL CHANGE: DO NOT CALL BAPI_TRANSACTION_COMMIT !!!
      " RAP will trigger the commit automatically at the end of the request
      " if the interaction phase finishes successfully.
      
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
    ENDIF.

  ENDMETHOD.
