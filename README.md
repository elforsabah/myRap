Description	Resource	Path	Location	Type
The association _ewa_order_object cannot be used locally in the view (see long text)	ZZPD_R_PDSERVICEEXTCUSTOM (Data Definition)	.adt/ddic/ddlsources/zzpd_r_pdserviceextcustom	line 6	ABAP Syntax Check Problem


extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association [0..1] to  ewa_order_object as _ewa_order_object on $projection.ZZPOBJNR = _ewa_order_object.pobjnr
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _ewa_order_object.zzpoo_wdoi as wdoi,
  _ZZOrderObject,
  _ewa_order_object
}

