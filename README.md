METHOD createlanf.

    " Data Declarations for EML
    DATA: lt_so_create      TYPE TABLE FOR CREATE I_SalesOrderTP,
          lt_so_items       TYPE TABLE FOR CREATE I_SalesOrderTP\_Item,
          lt_so_partners    TYPE TABLE FOR CREATE I_SalesOrderTP\_Partner,
          ls_so_create      LIKE LINE OF lt_so_create,
          ls_so_item        LIKE LINE OF lt_so_items,
          ls_so_partner     LIKE LINE OF lt_so_partners.

    " 1. Get Input
    READ TABLE keys INTO DATA(ls_key) INDEX 1.
    DATA(ls_input) = ls_key-%param.

    " 2. Read Contract Header (Using CDS View I_SalesContract)
    " We use ALPHA formatting to ensure the contract number is correct
    DATA(lv_contract) = |{ ls_input-contractvbeln ALPHA = IN }|.

    SELECT SINGLE SalesOrganization, DistributionChannel, OrganizationDivision
      FROM I_SalesContract
      WHERE SalesContract = @lv_contract
      INTO @DATA(ls_contract_data).

    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Contract { lv_contract } not found| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
      RETURN.
    ENDIF.

    " 3. Prepare Header Data (Root Entity)
    " We use a Content ID (%cid) to link the header to items/partners locally
    DATA(lv_cid) = 'HEADER_1'.

    ls_so_create = VALUE #(
      %cid                     = lv_cid
      %data-SalesOrderType     = 'ZLRA'  " Your Call-off Type
      %data-SalesOrganization  = ls_contract_data-SalesOrganization
      %data-DistributionChannel = ls_contract_data-DistributionChannel
      %data-OrganizationDivision = ls_contract_data-OrganizationDivision
      %data-SoldToParty        = ls_input-CustomerRef " Or derive from contract partners
      %data-PurchaseOrderByCustomer = ls_input-CustomerRef
      %data-RequestedDeliveryDate = ls_input-DeliveryDate
    ).
    APPEND ls_so_create TO lt_so_create.

    " 4. Prepare Partners
    " Read partners from the contract to copy them to the Order
    SELECT SalesContract, PartnerFunction, Customer, Supplier, Personnel
      FROM I_SalesContractPartner
      WHERE SalesContract = @lv_contract
      INTO TABLE @DATA(lt_contract_partners).

    LOOP AT lt_contract_partners INTO DATA(ls_db_partner).
      " Map Contract Partners to Order Partners
      ls_so_partner = VALUE #(
        %cid_ref                = lv_cid " Link to the header above
        %target = VALUE #( (
          %cid                  = |PARTNER_{ sy-tabix }|
          PartnerFunction       = ls_db_partner-PartnerFunction
          Customer              = ls_db_partner-Customer
          Supplier              = ls_db_partner-Supplier
          Personnel             = ls_db_partner-Personnel
        ) )
      ).
      APPEND ls_so_partner TO lt_so_partners.
    ENDLOOP.

    " 5. Prepare Items
    " Read Contract Items to find the correct Item Number for the Material
    SELECT SalesContractItem, Product, OrderQuantityUnit
      FROM I_SalesContractItem
      WHERE SalesContract = @lv_contract
      INTO TABLE @DATA(lt_contract_items).

    LOOP AT ls_input-_Positions INTO DATA(ls_pos_input).
      " Find matching item in contract
      DATA(lv_matnr) = |{ ls_pos_input-Matnr ALPHA = IN }|.

      READ TABLE lt_contract_items INTO DATA(ls_contract_item)
        WITH KEY Product = lv_matnr.

      IF sy-subrc = 0.
        ls_so_item = VALUE #(
          %cid_ref = lv_cid
          %target  = VALUE #( (
             %cid                 = |ITEM_{ sy-tabix }|
             Product              = lv_matnr
             RequestedQuantity    = ls_pos_input-Menge
             RequestedQuantityUnit = ls_pos_input-Meins
             " REFERENCE TO CONTRACT
             SalesContract        = lv_contract
             SalesContractItem    = ls_contract_item-SalesContractItem
          ) )
        ).
        APPEND ls_so_item TO lt_so_items.
      ELSE.
        " Material not found in contract -> Error
        APPEND VALUE #( %msg = new_message( id = '00' number = '001' v1 = |Material { ls_pos_input-Matnr } not in contract| severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
      ENDIF.
    ENDLOOP.

    IF reported-lanfroot IS NOT INITIAL.
       failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
       RETURN.
    ENDIF.

    " 6. EXECUTE EML (The "Magic" Step)
    " This replaces CALL FUNCTION 'BAPI...'
    MODIFY ENTITIES OF I_SalesOrderTP
      ENTITY SalesOrder
        CREATE FROM lt_so_create
        CREATE BY \_Item FROM lt_so_items
        CREATE BY \_Partner FROM lt_so_partners
      MAPPED DATA(ls_mapped)
      FAILED DATA(ls_failed)
      REPORTED DATA(ls_reported).

    " 7. Handle Success (Mapping)
    IF ls_failed IS INITIAL.
      " The new Order Number is in ls_mapped-salesorder
      READ TABLE ls_mapped-salesorder INTO DATA(ls_new_so) INDEX 1.
      DATA(lv_new_vbeln) = ls_new_so-SalesOrder.

      " Optional: If this is an unmanaged save, trigger the save sequence
      " (In a fully managed RAP scenario, the framework does this automatically)
      " COMMIT ENTITIES BEGIN RESPONSE OF I_SalesOrderTP FAILED DATA(ls_save_failed) REPORTED DATA(ls_save_reported).
      " COMMIT ENTITIES END. 
      " Note: Since you are likely in a custom action, you might need to rely on the framework's save phase 
      " or issue an explicit commit if this is a standalone utility.

      result = VALUE #(
        ( %param = VALUE #(
            techkey   = 'X'
            vbeln     = lv_new_vbeln
            _messages = VALUE #( ( msgid = '00' msgno = '000' msgv1 = 'Success' ) )
          ) ) ).

    ELSE.
      " 8. Handle Errors
      " Loop through RAP reported structure and convert to your message structure
      LOOP AT ls_reported-salesorder INTO DATA(ls_rep_so).
         APPEND VALUE #(
           %msg = ls_rep_so-%msg
         ) TO reported-lanfroot.
      ENDLOOP.
      
      " Also check item errors
      LOOP AT ls_reported-salesorderitem INTO DATA(ls_rep_item).
         APPEND VALUE #(
           %msg = ls_rep_item-%msg
         ) TO reported-lanfroot.
      ENDLOOP.

      failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    ENDIF.

  ENDMETHOD.
