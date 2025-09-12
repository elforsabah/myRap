@AbapCatalog.viewEnhancementCategory: [#NONE]  // Adjust based on base view; this is optional if not needed

extend view entity /PLCE/R_PDServiceExtCustom 
with 
  association [1..1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject 
  on _Service.ReferenceInternalId = _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _ZZOrderObject.ZZPOO_WDOI as sdd,
  _ZZOrderObject
}
