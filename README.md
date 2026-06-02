@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service Entsorgungsanlage with fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM
  as select from /plce/tpdsrv as _srv
  left outer join /plce/tpdsrvcst as _custom
    on _custom.service_uuid = _srv.service_uuid
  left outer join ewa_order_object as _order
    on  _order.pobjnr    = _srv.reference_int_id
    and _order.wdplantnr <> ''
{
  key _srv.service_uuid             as ServiceUUID,

  " ✅ MAX + GROUP BY = exactly ONE row per ServiceUUID
  max( _custom.wdplantnr )          as wdplantnr_custom,
  max( _order.wdplantnr  )          as wdplantnr_order
}
group by _srv.service_uuid
