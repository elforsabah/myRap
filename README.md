extend view entity /PLCE/R_PDServiceExtCustom with
  association [0..1] to ZI_WR_SERVICE_EXTCUSTOM as _ExtCustomHelper
    on _ExtCustomHelper.ServiceUUID = $projection.ServiceUUID
{
  /plce/tpdsrvcst.zz_tech_fachbe,
  /plce/tpdsrvcst.zz_discrepancy,
  /plce/tpdsrvcst.zz_reactiontime,
  /plce/tpdsrvcst.zz_vehicleinfo,
  /plce/tpdsrvcst.zz_order_date,
  /plce/tpdsrvcst.zz_timeadjustment,
  /plce/tpdsrvcst.zz_pobjnr_main,
  /plce/tpdsrvcst.wdplantnr,

  " ✅ Custom value (updated by anlageaendern)
  _ExtCustomHelper.wdplantnr_custom as wdplantnr_custom,

  " ✅ Order object fallback value
  _ExtCustomHelper.wdplantnr_order  as wdplantnr_order
}
