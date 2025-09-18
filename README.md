extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on   _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association [1] to /plce/tpdsrv as _tpdsrv  on  _tpdsrv.service_uuid  = $projection.ServiceUUID
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _tpdsrv,
  _ZZOrderObject
}

