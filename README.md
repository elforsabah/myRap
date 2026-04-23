  // ✅ NEW: Use CASE instead of COALESCE
  @EndUserText.label: 'Entsorgungsanlage'
  case
    when /PLCE/R_PDService._ExtCustom.wdplantnr is not initial
      then /PLCE/R_PDService._ExtCustom.wdplantnr
    else /PLCE/R_PDService._ExtCustom._ORDER_OBJECT.Entsorgunganlage
  end as zz_entsorgunganlage,
   
