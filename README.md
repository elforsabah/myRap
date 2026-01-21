" Map positions (filter Menge > 0, map to contract POSNR)
    map_positions_to_contract(
      iv_contract_vbeln = ls_input-contractvbeln
      it_positions      = CORRESPONDING #( ls_input-_positions )
      CHANGING ct_order_items  = lt_items_in
               ct_order_itemsx = lt_items_inx
               ct_return       = lt_return ).
