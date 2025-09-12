@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Custom Extension with WDOI Join'
extend view entity /PLCE/R_PDServiceExtCustom
  // Keep if needed for navigation; remove if not
  association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject
    on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
// Remove the association to EWA_ORDER_OBJECT entirely
{
  // Keep for reference/ON conditions if needed elsewhere
  _Service.ReferenceInternalId as ZZPOBJNR,
  
  // Direct join to project the field (replaces association)
  left outer join EWA_ORDER_OBJECT as _ewa_order_object
    on _ewa_order_object.POBJNR = $projection.ZZPOBJNR
  
  // Project the field directly from the join (supported in extensions)
  @EndUserText.label: 'WDOI Field'
  @Semantics.text: true  // Optional: If it's a text field
  _ewa_order_object.ZZPOO_WDOI as ZZPOBJ_WDOI
  
  // Optional: Expose _ZZOrderObject for navigation if required
  // _ZZOrderObject
}
