extend view entity /PLCE/R_PDServiceExtCustom  with
association [1] to ZI_WR_EWA_ORDER_OBJECT as _ewa_order_object on  $projection.ServiceUUID = _ewa_order_object.PdServiceUuid 
 {
    _ewa_order_object.PdServiceUuid
}

Unspecified provider error occurred. See Error Context and Call Stack.
