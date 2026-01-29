CLASS lhc_lanfroot DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    TYPES tt_lanf_position_input TYPE STANDARD TABLE OF zi_lanf_position_input WITH DEFAULT KEY.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR lanfroot RESULT result.

    METHODS read FOR READ
      IMPORTING keys FOR READ lanfroot RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK lanfroot.

    METHODS attachdocument FOR MODIFY
      IMPORTING keys FOR ACTION  lanfroot~attachdocument RESULT result.

    METHODS createlanf FOR MODIFY
      IMPORTING keys FOR ACTION lanfroot~createlanf RESULT result.

    METHODS map_positions_to_contract
      IMPORTING iv_contract_vbeln TYPE vbeln_va
                it_positions      TYPE tt_lanf_position_input
      CHANGING  ct_order_items    TYPE  bapisditm_tt
                ct_order_itemsx   TYPE  bapisditmx_tt
                ct_return         TYPE bapiret2_tab.


ENDCLASS.

CLASS lhc_lanfroot IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.

*METHOD createlanf.
*
*    " Data Declarations for EML
*    DATA: lt_so_create      TYPE TABLE FOR CREATE I_SalesOrderTP,
*          lt_so_items       TYPE TABLE FOR CREATE I_SalesOrderTP\_Item,
*          lt_so_partners    TYPE TABLE FOR CREATE I_SalesOrderTP\_Partner,
*          ls_so_create      LIKE LINE OF lt_so_create,
*          ls_so_item        LIKE LINE OF lt_so_items,
*          ls_so_partner     LIKE LINE OF lt_so_partners.
*
*    " 1. Get Input
*    READ TABLE keys INTO DATA(ls_key) INDEX 1.
*    DATA(ls_input) = ls_key-%param.
*
*    " 2. Read Contract Header (Using CDS View I_SalesContract)
*    " We use ALPHA formatting to ensure the contract number is correct
*    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.
*
*    SELECT SINGLE SalesOrganization, DistributionChannel, OrganizationDivision
*      FROM I_SalesContract
*      WHERE SalesContract = @lv_contract
*      INTO @DATA(ls_contract_data).
*
*    IF sy-subrc <> 0.
*      APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Contract { lv_contract } not found| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*    ENDIF.
*
*    " 3. Prepare Header Data (Root Entity)
*    " We use a Content ID (%cid) to link the header to items/partners locally
*    DATA(lv_cid) = 'HEADER_1'.
*
*    ls_so_create = VALUE #(
*      %cid                     = lv_cid
*      %data-SalesOrderType     = 'ZLRA'  " Your Call-off Type
*      %data-SalesOrganization  = ls_contract_data-SalesOrganization
*      %data-DistributionChannel = ls_contract_data-DistributionChannel
*      %data-OrganizationDivision = ls_contract_data-OrganizationDivision
*      %data-SoldToParty        = ls_input-CustomerRef " Or derive from contract partners
*      %data-PurchaseOrderByCustomer = ls_input-CustomerRef
*      %data-RequestedDeliveryDate = ls_input-DeliveryDate
*    ).
*    APPEND ls_so_create TO lt_so_create.
*
*    " 4. Prepare Partners
*    " Read partners from the contract to copy them to the Order
*    SELECT SalesContract, PartnerFunction, Customer, Supplier, Personnel
*      FROM I_SalesContractPartner
*      WHERE SalesContract = @lv_contract
*      INTO TABLE @DATA(lt_contract_partners).
*
*    LOOP AT lt_contract_partners INTO DATA(ls_db_partner).
*      " Map Contract Partners to Order Partners
*      ls_so_partner = VALUE #(
*        %cid_ref                = lv_cid " Link to the header above
*        %target = VALUE #( (
*          %cid                  = |PARTNER_{ sy-tabix }|
*          PartnerFunction       = ls_db_partner-PartnerFunction
*          Customer              = ls_db_partner-Customer
*          Supplier              = ls_db_partner-Supplier
*          Personnel             = ls_db_partner-Personnel
*        ) )
*      ).
*      APPEND ls_so_partner TO lt_so_partners.
*    ENDLOOP.
*
*    " 5. Prepare Items
*    " Read Contract Items to find the correct Item Number for the Material
*    SELECT SalesContractItem, Product, OrderQuantityUnit
*      FROM I_SalesContractItem
*      WHERE SalesContract = @lv_contract
*      INTO TABLE @DATA(lt_contract_items).
*
*    LOOP AT ls_input-_Positions INTO DATA(ls_pos_input).
*      " Find matching item in contract
*      DATA(lv_matnr) = |{ ls_pos_input-Matnr ALPHA = IN }|.
*
*      READ TABLE lt_contract_items INTO DATA(ls_contract_item)
*        WITH KEY Product = lv_matnr.
*
*      IF sy-subrc = 0.
*    ls_so_item = VALUE #(
*              %cid_ref = lv_cid
*              %target  = VALUE #( (
*                 %cid                  = |ITEM_{ sy-tabix }|
*                 Product               = lv_matnr
*                 RequestedQuantity     = ls_pos_input-Menge
*                 RequestedQuantityUnit = ls_pos_input-Meins
*
*                 " KORREKTUR: Referenz auf Kontrakt (Standard-Feldnamen)
**                 ReferenceSDDocument         = lv_contract
**                 ReferenceSDDocumentItem     = ls_contract_item-SalesContractItem
**                 ReferenceSDDocumentCategory = 'G'  " 'G' steht für Kontrakt
*              ) )
*            ).
*        APPEND ls_so_item TO lt_so_items.
*      ELSE.
*        " Material not found in contract -> Error
*        APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Material { ls_pos_input-Matnr } not in contract| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      ENDIF.
*    ENDLOOP.
*
*    IF reported-lanfroot IS NOT INITIAL.
*       failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*       RETURN.
*    ENDIF.
*
*    " 6. EXECUTE EML (The "Magic" Step)
*    " This replaces CALL FUNCTION 'BAPI...'
*    MODIFY ENTITIES OF I_SalesOrderTP
*      ENTITY SalesOrder
*        CREATE FROM lt_so_create
*        CREATE BY \_Item FROM lt_so_items
*        CREATE BY \_Partner FROM lt_so_partners
*      MAPPED DATA(ls_mapped)
*      FAILED DATA(ls_failed)
*      REPORTED DATA(ls_reported).
*
*    " 7. Handle Success (Mapping)
*    IF ls_failed IS INITIAL.
*      " The new Order Number is in ls_mapped-salesorder
*      READ TABLE ls_mapped-salesorder INTO DATA(ls_new_so) INDEX 1.
*      DATA(lv_new_vbeln) = ls_new_so-SalesOrder.
*
*      " Optional: If this is an unmanaged save, trigger the save sequence
*      " (In a fully managed RAP scenario, the framework does this automatically)
*      " COMMIT ENTITIES BEGIN RESPONSE OF I_SalesOrderTP FAILED DATA(ls_save_failed) REPORTED DATA(ls_save_reported).
*      " COMMIT ENTITIES END.
*      " Note: Since you are likely in a custom action, you might need to rely on the framework's save phase
*      " or issue an explicit commit if this is a standalone utility.
*
*      result = VALUE #(
*        ( %param = VALUE #(
*            techkey   = 'X'
*            vbeln     = lv_new_vbeln
*            _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success' ) )
*          ) ) ).
*
*    ELSE.
*      " 8. Handle Errors
*      " Loop through RAP reported structure and convert to your message structure
*      LOOP AT ls_reported-salesorder INTO DATA(ls_rep_so).
*         APPEND VALUE #(
*           %msg = ls_rep_so-%msg
*         ) TO reported-lanfroot.
*      ENDLOOP.
*
*      " Also check item errors
*      LOOP AT ls_reported-salesorderitem INTO DATA(ls_rep_item).
*         APPEND VALUE #(
*           %msg = ls_rep_item-%msg
*         ) TO reported-lanfroot.
*      ENDLOOP.
*
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*    ENDIF.
*
*  ENDMETHOD.


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
      lv_busobj         TYPE swo_objtyp,
      ls_item_in        TYPE bapisditm,
      ls_item_inx       TYPE bapisditmx.

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
    ls_header_in-doc_type   = 'ZLRA'.  " Check if this Order Type is correct
    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
    ls_header_in-division   = ls_contract_hdr-spart.
    ls_header_in-ref_doc    = lv_contract.
    ls_header_in-refdoc_cat = 'G'.
    ls_header_in-req_date_h = ls_input-deliverydate.
    ls_header_in-purch_no_c = ls_input-customerref.

    " Header Checkbox (INX)
    ls_header_inx-doc_type   = 'X'.
    ls_header_inx-sales_org  = 'X'.
    ls_header_inx-distr_chan = 'X'.
    ls_header_inx-division   = 'X'.
    ls_header_inx-ref_doc    = 'X'.
    ls_header_inx-refdoc_cat = 'X'.
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
           " Fallback: Try finding it without leading zeros if first attempt failed
           READ TABLE lt_contract_pos INTO ls_contr_item WITH KEY matnr = ls_pos_input-matnr.
        ENDIF.

        IF ls_contr_item-posnr IS INITIAL.
             APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Material { ls_pos_input-matnr } not found in Contract| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
             CONTINUE.
        ENDIF.

        CLEAR: ls_item_in, ls_item_inx.

        " --- DATA ---
        ls_item_in-itm_number  = ls_contr_item-posnr.
        ls_item_in-material    = lv_matnr_long.      " Short Field (Compat)
        ls_item_in-material_long = lv_matnr_long.    " Long Field (S/4HANA Requirement)
        ls_item_in-target_qty  = ls_pos_input-menge.
        ls_item_in-target_qu   = ls_pos_input-meins.
        ls_item_in-ref_doc     = lv_contract.
        ls_item_in-ref_doc_it  = ls_contr_item-posnr.
        ls_item_in-ref_doc_ca  = 'G'.

        " --- CHECKBOX (INX) ---
        ls_item_inx-itm_number = ls_contr_item-posnr.
        ls_item_inx-updateflag = 'I'.

        " Crucial: Mark BOTH short and long material as updated
        ls_item_inx-material   = ' '.
        ls_item_inx-material_long = 'X'.

        ls_item_inx-target_qty = 'X'.
        ls_item_inx-target_qu  = 'X'.
        ls_item_inx-ref_doc    = 'X'.
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
        order_header_in      = ls_header_in
        order_header_inx     = ls_header_inx
      IMPORTING
        salesdocument        = lv_vbeln
      TABLES
        return               = lt_return
        order_items_in       = lt_items_in
        order_items_inx      = lt_items_inx
        order_partners       = lt_partners.

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
      " Success!
      " RAP will trigger the database COMMIT automatically after this method finishes.

      result = VALUE #(
        ( %param = VALUE #(
            techkey   = 'X'
            vbeln     = lv_vbeln
            _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success' ) )
          ) ) ).
    ENDIF.

  ENDMETHOD.



*METHOD createlanf.
*
*    DATA:
*      " NOTE: Type changed from BAPISDHEAD to BAPISDHD1 for DAT2 BAPI
*      ls_header_in    TYPE bapisdhd1,
*      ls_header_inx   TYPE bapisdhd1x,
*
*      " These types now MATCH the BAPI perfectly (No conversion needed)
*      lt_items_in     TYPE bapisditm_tt,
*      lt_items_inx    TYPE bapisditmx_tt,
*      lt_partners     TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
*
*      lt_return       TYPE bapiret2_tab,
*      lv_vbeln        TYPE vbeln_va,
*      ls_contract_hdr TYPE vbak,
*      lv_vbtyp        TYPE tvak-vbtyp,
*      lv_busobj       TYPE swo_objtyp.
*
*    " 1. Get Input
*    READ TABLE keys INTO DATA(ls_key) INDEX 1.
*    DATA(ls_input) = ls_key-%param.
*
*    " 2. Validate Contract
*    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.
*    SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
*    IF sy-subrc <> 0.
*      APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Contract not found| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*    ENDIF.
*
*    " 3. Header Mapping (To BAPISDHD1)
*    ls_header_in-doc_type   = 'ZLRA'.
*    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
*    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
*    ls_header_in-division   = ls_contract_hdr-spart.
*    ls_header_in-ref_doc    = lv_contract.
*    ls_header_in-req_date_h = ls_input-deliverydate.
*    ls_header_in-purch_no_c = ls_input-customerref.
*
*    " Header X-Structure (Required by DAT2 BAPI for some fields)
*    ls_header_inx-doc_type   = 'X'.
*    ls_header_inx-sales_org  = 'X'.
*    ls_header_inx-distr_chan = 'X'.
*    ls_header_inx-division   = 'X'.
*    ls_header_inx-ref_doc    = 'X'.
**    ls_header_inx-ref_doc_ca = 'X'.
*    ls_header_inx-req_date_h = 'X'.
*    ls_header_inx-purch_no_c = 'X'.
*    ls_header_inx-updateflag = 'I'. " Insert
*
*    " 4. Partners
*    SELECT * FROM vbpa WHERE vbeln = @lv_contract INTO TABLE @DATA(lt_contract_partners).
*    lt_partners = CORRESPONDING #( lt_contract_partners MAPPING partn_role = parvw partn_numb = kunnr ).
*
*    " 5. Items (Direct Mapping - No manual loop needed!)
*    " The DAT2 BAPI uses BAPISDITM, which matches your helper method's output.
*    map_positions_to_contract(
*      EXPORTING
*        iv_contract_vbeln = ls_input-ContractVbeln
*        it_positions      = CORRESPONDING #( ls_input-_positions )
*      CHANGING
*        ct_order_items    = lt_items_in
*        ct_order_itemsx   = lt_items_inx
*        ct_return         = lt_return ).
*
*    IF line_exists( lt_return[ type = 'E' ] ).
*       " ... (Error handling logic same as before) ...
*       RETURN.
*    ENDIF.
*
*    " 6. Call the Modern BAPI
*    CALL FUNCTION 'BAPI_SALESORDER_CREATEFROMDAT2'
*      EXPORTING
*        order_header_in      = ls_header_in
*        order_header_inx     = ls_header_inx
*        " logic_switch       = ... (Optional: useful for pricing checks)
*      IMPORTING
*        salesdocument        = lv_vbeln
*      TABLES
*        return               = lt_return
*        order_items_in       = lt_items_in
*        order_items_inx      = lt_items_inx
*        order_partners       = lt_partners.
**         extensionin        = lt_extensionin. (Fully supported here if you need it!)
*
*    " 7. Handle Return
*    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
*       CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
*       " ... Map messages to reported ...
*    ELSE.
*       CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.
*       " ... Map success to result ...
*    ENDIF.
*
*  ENDMETHOD.




*METHOD createlanf.
*
*    DATA:
*      ls_header_in      TYPE bapisdhead,
*
*      " -- WRONG TYPES (Used by your helper method) --
*      lt_items_helper   TYPE bapisditm_tt,
*      lt_items_inx      TYPE bapisditmx_tt,
*      lt_partners_temp  TYPE STANDARD TABLE OF bapiparnr WITH DEFAULT KEY,
*
*      " -- CORRECT TYPES (Required by BAPI_SALESDOCU_CREATEFROMDATA) --
*      lt_items_bapi     TYPE STANDARD TABLE OF bapiitemin,
*      lt_partners_bapi  TYPE STANDARD TABLE OF bapipartnr,
*
*      lt_return         TYPE bapiret2_tab,
*      ls_return1        TYPE bapireturn1,
*      lv_vbeln          TYPE vbeln_va,
*      ls_contract_hdr   TYPE vbak,
*      lv_vbtyp          TYPE tvak-vbtyp,
*      lv_busobj         TYPE swo_objtyp.
*
*    " 1. Get Input Key
*    READ TABLE keys INTO DATA(ls_key) INDEX 1.
*    DATA(ls_input) = ls_key-%param.
*
*    " 2. Validate Contract
*    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.
*
*    SELECT SINGLE * FROM vbak INTO @ls_contract_hdr WHERE vbeln = @lv_contract.
*    IF sy-subrc <> 0.
*      APPEND VALUE #(
*        %msg = new_message(
*          id = '00' number = '001'
*          v1 = |Contract { lv_contract } not found|
*          severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*    ENDIF.
*
*    " 3. Determine Business Object
*    DATA(lv_auart) = 'ZLRA'.
*
*    SELECT SINGLE vbtyp FROM tvak INTO @lv_vbtyp WHERE auart = @lv_auart.
*    IF sy-subrc = 0 AND lv_vbtyp IS NOT INITIAL.
*      CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
*        EXPORTING i_document_type   = lv_vbtyp
*        IMPORTING e_business_object = lv_busobj.
*    ENDIF.
*    IF lv_busobj IS INITIAL.
*      lv_busobj = 'BUS2032'.
*    ENDIF.
*
*    " 4. Fill Header
*    CLEAR ls_header_in.
*    ls_header_in-doc_type   = lv_auart.
*    ls_header_in-sales_org  = ls_contract_hdr-vkorg.
*    ls_header_in-distr_chan = ls_contract_hdr-vtweg.
*    ls_header_in-division   = ls_contract_hdr-spart.
*    ls_header_in-ref_doc    = lv_contract.
*    ls_header_in-ref_doc_ca = 'G'.
*    ls_header_in-req_date_h = ls_input-deliverydate.
*    ls_header_in-purch_no_c = ls_input-customerref.
*
*    " 5. Fill Partners (Intermediate Step)
*    SELECT * FROM vbpa
*      WHERE vbeln = @lv_contract
*      INTO TABLE @DATA(lt_contract_partners).
*
*    lt_partners_temp = CORRESPONDING #( lt_contract_partners
*      MAPPING partn_role = parvw
*              partn_numb = kunnr ).
*
*    " >>> FIX: Convert to correct BAPI structure (BAPIPARTNR)
*    lt_partners_bapi = CORRESPONDING #( lt_partners_temp ).
*
*    " 6. Fill Items (Intermediate Step)
*    CLEAR: lt_items_helper, lt_items_inx.
*
*    " Your helper method returns BAPISDITM (which causes the crash)
*    map_positions_to_contract(
*      EXPORTING
*        iv_contract_vbeln = ls_input-ContractVbeln
*        it_positions      = CORRESPONDING #( ls_input-_positions )
*      CHANGING
*        ct_order_items    = lt_items_helper
*        ct_order_itemsx   = lt_items_inx
*        ct_return         = lt_return ).
*
*    IF line_exists( lt_return[ type = 'E' ] ) OR line_exists( lt_return[ type = 'A' ] ).
*      LOOP AT lt_return ASSIGNING FIELD-SYMBOL(<m>) WHERE type CA 'EA'.
*        APPEND VALUE #(
*          %msg = new_message(
*            id = <m>-id number = <m>-number
*            v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
*            severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      ENDLOOP.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*    ENDIF.
*
*    " >>> FIX: Convert Items to correct BAPI structure (BAPIITEMIN)
*    LOOP AT lt_items_helper ASSIGNING FIELD-SYMBOL(<helper_item>).
*      APPEND VALUE bapiitemin(
*        itm_number = <helper_item>-itm_number
*        material   = <helper_item>-material
*        target_qty = <helper_item>-target_qty
*        target_qu  = <helper_item>-target_qu
*        ref_doc    = <helper_item>-ref_doc
*        ref_doc_it = <helper_item>-ref_doc_it
*        ref_doc_ca = <helper_item>-ref_doc_ca
*      ) TO lt_items_bapi.
*    ENDLOOP.
*
*    " 7. Call BAPI with CORRECTED tables
*    CALL FUNCTION 'BAPI_SALESDOCU_CREATEFROMDATA'
*      EXPORTING
*        order_header_in = ls_header_in
*        business_object = lv_busobj
*        without_commit  = abap_true
*      IMPORTING
*        salesdocument   = lv_vbeln
*        return          = ls_return1
*      TABLES
*        order_items_in  = lt_items_bapi    " Type BAPIITEMIN
*        order_partners  = lt_partners_bapi. " Type BAPIPARTNR
*
*    " 8. Handle BAPI Return
*    APPEND VALUE bapiret2(
*      type       = ls_return1-type
*      id         = ls_return1-id
*      number     = ls_return1-number
*      message    = ls_return1-message
*      message_v1 = ls_return1-message_v1
*      message_v2 = ls_return1-message_v2
*      message_v3 = ls_return1-message_v3
*      message_v4 = ls_return1-message_v4
*    ) TO lt_return.
*
*    IF ls_return1-type CA 'EAX'.
**      CALL FUNCTION 'BAPI_TRANSACTION_ROLLBACK'.
*      LOOP AT lt_return ASSIGNING <m> WHERE type CA 'EAX'.
*        APPEND VALUE #(
*          %msg = new_message(
*            id = <m>-id number = <m>-number
*            v1 = <m>-message_v1 v2 = <m>-message_v2 v3 = <m>-message_v3 v4 = <m>-message_v4
*            severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
*      ENDLOOP.
*      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
*      RETURN.
*    ENDIF.
*
**    CALL FUNCTION 'BAPI_TRANSACTION_COMMIT' EXPORTING wait = abap_true.
*
*    result = VALUE #(
*      ( %param = VALUE #(
*          techkey   = 'X'
*          vbeln     = lv_vbeln
*          _messages = VALUE #(
*            FOR r IN lt_return (
*              techkey = 'X'
*              msgid   = r-id
*              msgno   = r-number
*              msgty   = r-type
*              msgv1   = r-message_v1
*              msgv2   = r-message_v2
*              msgv3   = r-message_v3
*              msgv4   = r-message_v4 ) ) ) ) ).
*
*  ENDMETHOD.



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



ENDCLASS.

CLASS lsc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.

    METHODS finalize REDEFINITION.

    METHODS check_before_save REDEFINITION.

    METHODS save REDEFINITION.

    METHODS cleanup REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_zi_lanf_root IMPLEMENTATION.

  METHOD finalize.
  ENDMETHOD.

  METHOD check_before_save.
  ENDMETHOD.

  METHOD save.
  ENDMETHOD.

  METHOD cleanup.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.
