@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service Entsorgungsanlage with fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM

  " ✅ Primary: ExtCustom table
  as select from /plce/tpdsrvcst as _custom

  " ✅ Bridge to get pobjnr
  inner join /plce/tpdsrv as _srv
    on _srv.service_uuid = _custom.service_uuid

  " ✅ Fallback: Order object at TABLE level (not association!)
  left outer join ewa_order_object as _order
    on  _order.pobjnr    = _srv.reference_int_id
    and _order.wdplantnr <> ''
{
  key _custom.service_uuid as ServiceUUID,

  " ✅ COALESCE works here - both are real table fields!
  coalesce( _custom.wdplantnr, _order.wdplantnr ) as wdplantnr
}


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

  " ✅ No COALESCE here - just expose the pre-computed result!
  _ExtCustomHelper.wdplantnr as order_wdplantnr
}
