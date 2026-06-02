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
