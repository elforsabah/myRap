BEGIN OF ty_bms_order,
  order        TYPE string,    " ← add this
  status       TYPE string,
  order_number TYPE string,
  ...
END OF ty_bms_order,


DATA(ls_order) = VALUE ty_bms_order(
  order        = |{ ls_ewa-smaufnr ALPHA = OUT }|   " ← add
  order_number = |{ ls_ewa-smaufnr ALPHA = OUT }|
  ...
