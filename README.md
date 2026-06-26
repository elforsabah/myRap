item_price = COND decfloat16(
  WHEN ls_vbap-netpr IS INITIAL THEN 0
  ELSE ls_vbap-netpr )
