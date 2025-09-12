@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'PD Service Extension Custom - Waste Order Link'
extend view entity /PLCE/R_PDServiceExtCustom with
// Define associations (for navigation; not used locally)
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject 
  on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association [0..1] to ewa_order_object as _ewa_order_object 
  on $projection.ZZPOBJNR = _ewa_order_object.pobjnr  // Cardinality [0..1] assumes optional; change to [1] if mandatory
{
  // New elements: Project parent field, join for field access (avoids local assoc usage), expose associations
  @EndUserText.label: 'Waste Order Object Number'
  _Service.ReferenceInternalId as ZZPOBJNR,

  // Direct join to expose zzpoo_wdoi (replaces local path; adjust LEFT OUTER JOIN if optional data)
  @EndUserText.label: 'WDOI Value'  // Adjust label as needed
  @Consumption.valueHelpDefinition: [{  // Optional: Value help if zzpoo_wdoi has a domain
    entity: { name: 'ewa_order_object', element: 'zzpoo_wdoi' }
  }]
  @Consumption.filter.hidden: false  // Optional: Allow filtering if needed
  wdoi : joined(
    // Use INNER JOIN if always present; LEFT OUTER JOIN for optional
    [left outer] join ewa_order_object as _ewa_join on _ewa_join.pobjnr = $projection.ZZPOBJNR
    { _ewa_join.zzpoo_wdoi }
  ) as wdoi,  // Alias if needed; exposes as 'wdoi' in the extended entity

  // Expose associations for external use (e.g., navigation in Fiori)
  _ZZOrderObject,
  _ewa_order_object
}
