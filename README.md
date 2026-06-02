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
  key _srv.service_uuid as ServiceUUID,

  " ✅ wdplantnr with fallback
  max( case
    when _custom.wdplantnr is not initial
      then _custom.wdplantnr
    else _order.wdplantnr
  end ) as wdplantnr,

  " ✅ zz_pobjnr_main with fallback + strip WE prefix
  max( case
    when cast( _custom.zz_pobjnr_main as abap.int8 ) > 0
      then _custom.zz_pobjnr_main   " Already clean '346776' format

    when _order.zz_pobjnr_main is not initial
      then cast(
             cast(
               " ✅ KEY FIX: Remove 'WE' prefix first, THEN cast to int8
               " Works for ANY length: WE000...346776 or WE000...008576
               replace( rtrim( _order.zz_pobjnr_main, ' ' ), 'WE', '' )
             as abap.int8 )
           as abap.char( 20 ) )
    else ''
  end ) as zz_pobjnr_main
}
group by _srv.service_uuid
