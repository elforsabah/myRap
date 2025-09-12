extend view entity /PLCE/C_PDMNLServiceWR with
{
  association [0..1] to ZI_WR_EWA_ORDER_OBJECT as _OrderObject on _OrderObject.ServiceUUID = $projection.ServiceUUID;
  _OrderObject.point_of_origin as point_of_origin2 : abap.char(20)
}
