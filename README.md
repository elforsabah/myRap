extend view entity /PLCE/R_PDServiceExtCustom with 
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _ZZOrderObject.ZZPOO_WDOI as sdd,
  _ZZOrderObject
}



Description	Resource	Path	Location	Type
The association _ZZOrderObject cannot be used locally in the view (see long text)	ZZPD_R_PDSERVICEEXTCUSTOM (Data Definition)	/S4D_100_prologaefo_en/.adt/ddic/ddlsources/zzpd_r_pdserviceextcustom	line 5	ABAP Syntax Check Problem
