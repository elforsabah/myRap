extend view entity /PLCE/R_PDTourExtCustom with {
   /plce/tpdtourcst.zz_remaining_duration,
   /plce/tpdtourcst.zz_bms_status
}



extend view entity /PLCE/C_PDMNLTourWR with {
   /PLCE/R_PDTour._ExtCustom.zz_bms_status , // BMS Status
 
  @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
  @EndUserText.label: 'BMS Criticality'
  @UI.hidden: true
  virtual ZzBmsCriticality : abap.int1,
 
   // 1. Used Capacity (MUST BE DEC FOR PROGRESS BAR TO WORK)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
//   @EndUserText.label: 'Kapazitätsauslastung'
 
//   @UI.hidden: true
   virtual tour_capacity_new : abap.dec( 10, 2 ), 

   // 2. Total Duration (TARGET - Link Unit HERE)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit' 
//   @UI.hidden: true
   virtual tour_duration_min : abap.quan( 10, 2 ),      

   // 3. Unit Field (Holds 'MIN')
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.unitOfMeasure: true
   @UI.hidden: true
   virtual zz_duration_unit : abap.unit( 3 ),

   // 4. Remaining Duration (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
//   @EndUserText.label: 'Restdauer'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
//   @UI.hidden: true
   virtual zz_remaining_duration : abap.quan( 10, 2 ),
   
   
   // 4. MaximumTourDuration converted (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
//   @EndUserText.label: 'Converted MaximumTourDuration '
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
   @UI.hidden: true
   virtual MaximumTourDuration_Converted : abap.quan( 10, 2 ),
   
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @UI.hidden: true
   virtual zz_capacity_criticality : abap.int1,
   
   // Add this to your existing extension:
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Fahrzeit'  // Driving Time
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
   virtual zz_driving_time_min : abap.quan( 10, 2 )
   
}
 
