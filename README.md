

extend view entity /PLCE/R_PDServiceExtCustom with
 association [0..1] to ZI_WR_SERVICE_EXTCUSTOM as _ExtCustomHelper
    on _ExtCustomHelper.ServiceUUID = $projection.ServiceUUID
    
 association [0..1] to ZI_WR_EWA_ORDER_OBJECT as _ewatr on _ewatr.ServiceUUID =  $projection.ServiceUUID
    
{
  /plce/tpdsrvcst.zz_tech_fachbe,
  /plce/tpdsrvcst.zz_discrepancy,
  /plce/tpdsrvcst.zz_reactiontime,
  /plce/tpdsrvcst.zz_vehicleinfo,
  /plce/tpdsrvcst.zz_order_date,
  /plce/tpdsrvcst.zz_timeadjustment,
  /plce/tpdsrvcst.zz_pobjnr_main,
 
   coalesce( /plce/tpdsrvcst.wdplantnr, _ewatr.wdplantnr ) as order_wdplantnr,
 
 
 _ewatr.wdplantnr as order_wdplantnrr
}
