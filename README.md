extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = _Service.ReferenceInternalId
{
_Service.ReferenceInternalId as ZZPOBJNR,
_ZZOrderObject.ZZPOO_WDOI as sdd,
_ZZOrderObject
}
