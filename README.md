@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service Entsorgungsanlage with fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM
  as select from /plce/tpdsrv as _srv

  " ✅ LEFT OUTER: ExtCustom might not exist
  left outer join /plce/tpdsrvcst as _custom
    on _custom.service_uuid = _srv.service_uuid

  " ✅ LEFT OUTER: Order object fallback
  " Use reference_int_id directly from service table - no subquery needed!
  left outer join ewa_order_object as _order
    on  _order.pobjnr    = _srv.reference_int_id
    and _order.wdplantnr <> ''
{
  key _srv.service_uuid as ServiceUUID,

  " ✅ COALESCE: ExtCustom first → Order Object fallback
  coalesce( _custom.wdplantnr, _order.wdplantnr ) as wdplantnr
}
