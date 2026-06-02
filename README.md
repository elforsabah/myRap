" Step 1: Extend /PLCE/R_PDServiceExtCustom to expose order object wdplantnr
extend view entity /PLCE/R_PDServiceExtCustom with
association [0..1] to ZI_WR_EWA_ORDER_OBJECT as _ORDER_OBJECT 
  on _ORDER_OBJECT.PdServiceUuid = /plce/tpdsrvcst.service_uuid
{
  /plce/tpdsrvcst.zz_tech_fachbe,
  /plce/tpdsrvcst.zz_discrepancy,
  /plce/tpdsrvcst.zz_reactiontime,
  /plce/tpdsrvcst.zz_vehicleinfo,
  /plce/tpdsrvcst.zz_order_date,
  /plce/tpdsrvcst.zz_timeadjustment,
  /plce/tpdsrvcst.zz_pobjnr_main,
  /plce/tpdsrvcst.wdplantnr,
  
  " ✅ NEW: Expose order object wdplantnr with different alias
  @UI.hidden: true
  _ORDER_OBJECT.Entsorgunganlage as order_wdplantnr
}
