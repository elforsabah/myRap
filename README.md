map_positions_to_contract(
  EXPORTING
    iv_contract_vbeln = lv_contract_vbeln
    it_positions      = lt_positions
  CHANGING
    ct_order_items    = lt_order_items
    ct_order_itemsx   = lt_order_itemsx
    ct_return         = lt_return
).
