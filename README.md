extend view entity /PLCE/R_PDServiceExtCustom with {
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_PD_TOUR_DEVIATION_CALC'
  virtual TourDeviations : abap.int4;  // Use appropriate type; int4 for integer values like 4
}
