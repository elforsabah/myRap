" ✅ Get the record with WDPLANTNR - prefer latest or specific ORDER_LAUFNR
SELECT wdplantnr, zz_tech_fachbe, zz_pobjnr_main
  FROM ewa_order_object
  WHERE pobjnr = @<ls_service>-referenceinternalid
    AND wdplantnr IS NOT INITIAL        " ✅ Only get records WITH wdplantnr
  ORDER BY order_laufnr DESCENDING      " ✅ Get latest
  INTO @DATA(ls_order_fields)
  UP TO 1 ROWS.
ENDSELECT.
