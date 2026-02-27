// Add this to your existing extension:
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'
   @EndUserText.label: 'Fahrzeit'  // Driving Time
   @Semantics.quantity.unitOfMeasure: 'zz_duration_unit'
   virtual zz_driving_time_min : abap.quan( 10, 2 ),
