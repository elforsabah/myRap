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
//   coalesce( /plce/tpdsrvcst.wdplantnr, _ewatr.wdplantnr ) as order_wdplantnr,
 
 _ExtCustomHelper.wdplantnr as order_wdplantnr
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service Entsorgungsanlage with fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM
  as select from /plce/tpdsrv as _srv
  left outer join /plce/tpdsrvcst as _custom
    on _custom.service_uuid = _srv.service_uuid
  left outer join ZI_WR_EWA_ORDER_OBJECT as _order
    on  _order.ServiceUUID = _srv.service_uuid

{
  key _srv.service_uuid as ServiceUUID,

  coalesce( _custom.wdplantnr, _order.wdplantnr ) as wdplantnr
}
