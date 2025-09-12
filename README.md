extend view entity /PLCE/R_PDServiceExtCustom with
association [0..1] to ZI_WR_EWA_ORDER_OBJECT as _ewa_order_object on $projection.ServiceUUID = _ewa_order_object.PdServiceUuid
{
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_CALC_POINT_OF_ORIGIN'  // Reference your new class
  virtual point_of_origin : abap.char( 20 )  // Adjust type/length to match the actual field type in ZI_WR_EWA_ORDER_OBJECT
}
