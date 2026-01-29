ls_so_item = VALUE #(
          %cid_ref = lv_cid
          %target  = VALUE #( (
             %cid                  = |ITEM_{ sy-tabix }|
             Product               = lv_matnr
             RequestedQuantity     = ls_pos_input-Menge
             RequestedQuantityUnit = ls_pos_input-Meins
             
             " KORREKTUR: Referenz auf Kontrakt (Standard-Feldnamen)
             ReferenceSDDocument         = lv_contract
             ReferenceSDDocumentItem     = ls_contract_item-SalesContractItem
             ReferenceSDDocumentCategory = 'G'  " 'G' steht für Kontrakt
          ) )
        ).
        APPEND ls_so_item TO lt_so_items.
