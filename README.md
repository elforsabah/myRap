<img width="852" height="688" alt="image" src="https://github.com/user-attachments/assets/2ba9bd6e-2faf-4aa5-a551-734b51d251f6" />
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
 
//   coalesce( /plce/tpdsrvcst.wdplantnr, _ewatr.wdplantnr ) as order_wdplantnr,
 
 _ExtCustomHelper.wdplantnr as order_wdplantnr
}
