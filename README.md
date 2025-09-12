extend view entity /PLCE/R_PDServiceExtCustom with
  association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
  association [0..1] to EWA_ORDER_OBJECT as _ewa_order_object on $projection.ZZPOBJNR = _ewa_order_object.pobjnr
{
  _Service.ReferenceInternalId as ZZPOBJNR,  // Already added; keep for ON condition
  
  // Expose the field directly via path (supported here as it's not a projection view)
  @EndUserText.label: 'WDOI Field'
  _ewa_order_object.zzpoo_wdoi as ZZPOBJ_WDOI  // Direct projection of the field
  
}
