@AccessControl.authorizationCheck: #NOT_REQUIRED
@ClientHandling.type: #CLIENT_DEPENDENT
@ClientHandling.algorithm: #SESSION_VARIABLE
@EndUserText.label: 'Extended Work Area Service - Table Function'
define table function /PLCE/P_PDWorkAreaServiceFuncExt
  with parameters
    @Environment.systemField: #CLIENT
    p_client : abap.clnt
returns
{
  mandt : abap.clnt;
  work_area : /plce/pdwork_area;
  service_uuid : /plce/pdservice_uuid;
}
implemented by method
  /PLCE/CL_PD_WORKAREA_FUNC_EXT=>GET_WORKAREAS_SERVICES_EXT;  // New custom class/method
