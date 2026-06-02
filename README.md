@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Service ExtCustom with Entsorgungsanlage fallback'
define view entity ZI_WR_SERVICE_EXTCUSTOM
  as select from ZI_WR_EWA_ORDER_OBJECT as _custom
  
  inner join /plce/tpdsrv as _srv
    on _srv.service_uuid = _custom.ServiceUUID
    
  left outer join ewa_order_object as _order
    on _order.pobjnr = _srv.reference_int_id

{
  key _custom.ServiceUUID                as ServiceUUID,
  
  _custom.zz_tech_fachbe                  as zz_tech_fachbe,
  _custom.zz_reactiontime                 as zz_reactiontime,
  _custom.zz_order_date                   as zz_order_date,
  _custom.zz_pobjnr_main                  as zz_pobjnr_main,

  coalesce( _custom.wdplantnr, _order.wdplantnr ) as wdplantnr
}


extend view entity /PLCE/C_PDMNLServiceWR with
  association [0..1] to /PLCE/C_PDMNLTourWR as _TourForRefresh 
    on $projection.TourId = _TourForRefresh.TourId
{
  /PLCE/R_PDService._ExtCustom.zz_tech_fachbe,
  /PLCE/R_PDService._ExtCustom.zz_discrepancy,
   
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  virtual service_criticality : abap.int1,
  
  /PLCE/R_PDService._ExtCustom.zz_reactiontime,
  /PLCE/R_PDService._ExtCustom.zz_vehicleinfo,
  
  @UI.hidden: true
  /PLCE/R_PDService._ExtCustom.zz_timeadjustment,
   
  @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Zeitanpassung in Min'
  virtual zz_timeadjust_display : abap.char(35),
  
  @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition Kritikalität'
  virtual zz_main_pos_criticality : abap.int1,
  
  @EndUserText.label: 'Hauptposition'
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  @EndUserText.label: 'Entsorgungsanlage'
  /PLCE/R_PDService._ExtCustom.order_wdplantnr as wdplantnr,
   
  _TourForRefresh
}
