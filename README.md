
extend view entity /PLCE/C_PDMNLTourWR with {
   
   // 1. Used Capacity (Minutes)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Kapazitätsauslastung'
   virtual tour_capacity_new : abap.dec( 10, 0 ),

   // 2. Remaining Duration (Minutes)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Restdauer'
   virtual zz_remaining_duration : abap.int4,

   // 3. NEW: Total Duration in Minutes (Target for the Progress Bar)
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   virtual tour_duration_min : abap.int4
}
