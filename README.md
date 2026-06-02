@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service Entsorgungsanlage with fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM
  as select from /plce/tpdsrv as _srv
  left outer join /plce/tpdsrvcst as _custom
    on _custom.service_uuid = _srv.service_uuid
  left outer join ewa_order_object as _order
    on _order.pobjnr = _srv.reference_int_id
{
  key _srv.service_uuid as ServiceUUID,

  " ✅ wdplantnr: ExtCustom first, order object fallback
  max( case
    when _custom.wdplantnr is not initial
      then _custom.wdplantnr
    else _order.wdplantnr
  end ) as wdplantnr,

  " ✅ zz_pobjnr_main: ExtCustom first (already stripped by determination)
  " Fallback: strip 'WE000000000138817' → '138817'
  max( case
    when _custom.zz_pobjnr_main is not initial
      then _custom.zz_pobjnr_main   " Already '138817' format ✅
    when _order.zz_pobjnr_main is not initial
      then cast(
             cast(
               right( _order.zz_pobjnr_main, 16 )  " '0000000000138817'
             as abap.int8 )                          " 138817
           as abap.char( 16 ) )                      " '138817'
    else ''
  end ) as zz_pobjnr_main
}
group by _srv.service_uuid



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

  " ✅ wdplantnr with fallback
  _ExtCustomHelper.wdplantnr        as order_wdplantnr,

  " ✅ zz_pobjnr_main with fallback (already stripped)
  _ExtCustomHelper.zz_pobjnr_main   as order_zz_pobjnr_main
}
