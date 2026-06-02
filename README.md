" Step 2: In /PLCE/C_PDMNLServiceWR extension
" Use COALESCE: ExtCustom value first, fallback to order object value
@EndUserText.label: 'Entsorgungsanlage'
coalesce(
  /PLCE/R_PDService._ExtCustom.wdplantnr,        " User-changed value
  /PLCE/R_PDService._ExtCustom.order_wdplantnr    " Order object fallback
) as wdplantnr,
