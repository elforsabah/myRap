extend view entity /PLCE/C_PDMNLTourWR with {
   
   // 1. Used Capacity (Minutes)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Kapazitätsauslastung'
   virtual tour_capacity_new : abap.dec( 10, 0 ),

   // 2. Remaining Duration (LINKED TO UNIT)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Restdauer'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'  // <--- The Link
   virtual zz_remaining_duration : abap.quan( 10, 0 ),    // <--- Changed to QUAN

   // 3. New Virtual Unit Field
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.unitOfMeasure: true
   virtual zz_duration_unit : abap.unit( 3 ),             // <--- Holds 'MIN'

   // 4. Total Duration (Helper)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   virtual tour_duration_min : abap.int4
}
