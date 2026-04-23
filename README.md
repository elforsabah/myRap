coalesce( 
  /PLCE/R_PDService._ExtCustom.wdplantnr,  // ← First choice
  _OrderObject.entsorgunganlage             // ← Fallback
) as zz_entsorgunganlage
