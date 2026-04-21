extend view entity /PLCE/C_PDMNLServiceWR with

 // 1. DEFINE the Association (The Bridge)
   association [0..1] to /PLCE/C_PDMNLTourWR as _TourForRefresh 
      on $projection.TourId = _TourForRefresh.TourId
 {

   /PLCE/R_PDService._ExtCustom.zz_tech_fachbe, //as Fachberei
   /PLCE/R_PDService._ExtCustom.zz_discrepancy,  // Termin-Diskrepanz
   
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
   virtual service_criticality : abap.int1,
   /PLCE/R_PDService._ExtCustom.zz_reactiontime, // “Reaktionszeit
   /PLCE/R_PDService._ExtCustom.zz_vehicleinfo,  // Fahrzeuginfo
   @UI.hidden: true
   /PLCE/R_PDService._ExtCustom.zz_timeadjustment, // Zeitanpassung
   
   // 2. The NEW VIRTUAL field (Formatted Text). 
   // This will hold the "+37 MIN" string for the UI.
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
   @EndUserText.label: 'Zeitanpassung in Min'
   virtual zz_timeadjust_display : abap.char( 35 ),
   
  @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition Kritikalität'
  virtual zz_main_pos_criticality : abap.int1,
  
   @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition'
  virtual zz_main_position : zwr_d_pobjnr_main,
  
  @EndUserText.label: 'Hauptposition '
//  @UI.selectionField: [{ position: 1000 }]
   @UI.lineItem: [{ hidden: true }]
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  virtual zz_entsorgunganlage :  ewael_wdplantnr, // Entsorgungsanlage
  
   _TourForRefresh 
}
