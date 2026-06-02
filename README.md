@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service ExtCustom with Entsorgungsanlage fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM

  " ✅ Primary source: Service table (always has ALL services)
  as select from /plce/tpdsrv as _srv

  " ✅ LEFT OUTER: ExtCustom might not exist
  left outer join /plce/tpdsrvcst as _custom
    on _custom.service_uuid = _srv.service_uuid

  " ✅ LEFT OUTER: Order object might not exist
  left outer join ewa_order_object as _order
    on  _order.pobjnr    = _srv.reference_int_id
    and _order.wdplantnr <> ''
{
  key _srv.service_uuid  as ServiceUUID,

  _custom.zz_tech_fachbe    as zz_tech_fachbe,
  _custom.zz_reactiontime   as zz_reactiontime,
  _custom.zz_order_date     as zz_order_date,
  _custom.zz_pobjnr_main    as zz_pobjnr_main,

  " ✅ COALESCE: ExtCustom first → Order Object fallback
  coalesce( _custom.wdplantnr, _order.wdplantnr ) as wdplantnr
}
