extend view entity /PLCE/C_PDMNLTourWR with {
   
   // 1. Used Capacity (MUST BE DEC FOR PROGRESS BAR TO WORK)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Kapazitätsauslastung'
   // Do NOT link unit here, or the bar disappears!
   virtual tour_capacity_new : abap.dec( 10, 0 ), 

   // 2. Total Duration (TARGET - Link Unit HERE)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit' // <--- Trick: Link unit to the Target
   virtual tour_duration_min : abap.quan( 10, 0 ),       // <--- Change Target to QUAN

   // 3. Unit Field (Holds 'MIN')
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @Semantics.unitOfMeasure: true
   virtual zz_duration_unit : abap.unit( 3 ),

   // 4. Remaining Duration (Standard display)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Restdauer'
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
   virtual zz_remaining_duration : abap.quan( 10, 0 )
}
