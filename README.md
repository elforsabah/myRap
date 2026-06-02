 " ✅ CASE: Custom value wins, fallback to order object
  @EndUserText.label: 'Entsorgungsanlage'
  @UI.lineItem: [{ position: 132, importance: #HIGH }]
  case
    when /PLCE/R_PDService._ExtCustom.wdplantnr_custom is not initial
      then /PLCE/R_PDService._ExtCustom.wdplantnr_custom
    else /PLCE/R_PDService._ExtCustom.wdplantnr_order
  end as wdplantnr,
