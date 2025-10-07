extend view entity /PLCE/R_PDServiceExtCustom with 
//association[0..1] to ZE_P_PDWorkAreaServiceExt  as _WorkAreaServicesExt on $projection.ServiceUUID = _WorkAreaServicesExt.ServiceUUID
 {
/plce/tpdsrvcst.zz_tech_fachbe
//_WorkAreaServicesExt
}
