extend view entity /PLCE/C_PDMNLTourWR with {
//   /PLCE/R_PDTour._ExtCustom.zz_remaining_duration, // Restdauer
   
 
   // 1. Used Capacity (MUST BE DEC FOR PROGRESS BAR TO WORK)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Kapazitätsauslastung'
 
//   @UI.hidden: true
   virtual tour_capacity_new : abap.dec( 10, 0 ), 

   // 2. Total Duration (TARGET - Link Unit HERE)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit' 
//   @UI.hidden: true
   virtual tour_duration_min : abap.quan( 10, 0 ),      

   // 3. Unit Field (Holds 'MIN')
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.unitOfMeasure: true
   @UI.hidden: true
   virtual zz_duration_unit : abap.unit( 3 ),

   // 4. Remaining Duration (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Restdauer'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
//   @UI.hidden: true
   virtual zz_remaining_duration : abap.quan( 10, 2 ),
   
   
   // 4. MaximumTourDuration converted (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Converted MaximumTourDuration '
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
   @UI.hidden: true
   virtual MaximumTourDuration_Converted : abap.quan( 10, 3 )
}
 
