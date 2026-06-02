@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service ExtCustom with Entsorgungsanlage fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM
  as select from /plce/tpdsrvcst as _custom
  
  " ✅ FIX 1: Join service table first to get reference_int_id
  inner join /plce/tpdsrv as _srv
    on _srv.service_uuid = _custom.service_uuid
    
  " ✅ FIX 2: Then join order object using reference_int_id
  left outer join ewa_order_object as _order
    on _order.pobjnr = _srv.reference_int_id

{
  key _custom.service_uuid                as ServiceUUID,
  
  _custom.zz_tech_fachbe                  as zz_tech_fachbe,
  _custom.zz_discrepancy                  as zz_discrepancy,
  _custom.zz_reactiontime                 as zz_reactiontime,
  _custom.zz_vehicleinfo                  as zz_vehicleinfo,
  _custom.zz_order_date                   as zz_order_date,
  _custom.zz_timeadjustment               as zz_timeadjustment,
  _custom.zz_pobjnr_main                  as zz_pobjnr_main,

  " ✅ FIX 3: COALESCE with explicit table prefixes - no ambiguity
  coalesce( _custom.wdplantnr, _order.wdplantnr ) as wdplantnr
}
