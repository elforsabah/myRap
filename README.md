extend view entity /PLCE/C_PDMNLServiceWR with
  association [0..1] to /PLCE/C_PDMNLTourWR as _TourForRefresh 
    on $projection.TourId = _TourForRefresh.TourId

  " ✅ Add association to helper view
  association [0..1] to ZI_WR_SERVICE_EXTCUSTOM as _ExtCustomHelper
    on _ExtCustomHelper.ServiceUUID = $projection.ServiceUUID
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
  @UI.selectionField: [{ position: 1000 }]
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  " ✅ Read from helper view - COALESCE already handled there!
  @EndUserText.label: 'Entsorgungsanlage'
  @UI.lineItem: [{ position: 132, importance: #HIGH }]
  _ExtCustomHelper.wdplantnr as wdplantnr,
   
  _TourForRefresh
}
