
extend view entity /PLCE/C_PDMNLServiceWR with
association [0..1] to /PLCE/R_PDServiceExtCustom as _ExtCustom on $projection.ServiceUUID = _ExtCustom.ServiceUUID
{
  _ExtCustom._ewa_order_object.zzpoo_wdoi,
  _ExtCustom
}

Description	Resource	Path	Location	Type
Field selection of association _ExtCustom is not supported in Projection Views	ZC_PDMNLSERVICEWR (Data Definition)	.adt/ddic/ddlsources/zc_pdmnlservicewr	line 5	ABAP Syntax Check Problem
