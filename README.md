extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association to ewa_order_object as _ewa_order_object on $projection.ZZPOBJNR = _ewa_order_object.pobjnr 
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _ZZOrderObject,
  _ewa_order_object
}
