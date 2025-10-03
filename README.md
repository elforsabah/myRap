@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Work Areas Services'
@Search.searchable: false
define view entity Z_WORKAREAS_SERVICES
  with parameters
    P_Client : mandt
as select distinct from /PLCE/R_PDSERVICE as SRVC
  inner join /PLCE/R_PDWORKAREAPROFILE as WAPR
    on WAPR.PROFILE like SRVC.PROFILE
  inner join /PLCE/R_PDFUNCTIONALLOCATION as FL
    on FL.FUNCTIONALLOCATION = SRVC.FUNCTIONALLOCATION
  inner join /PLCE/R_PDWORKAREAPOSTALCODE as WAPC
    on  WAPC.WORKAREA = WAPR.WORKAREA
    and FL.POSTALCODE like WAPC.POSTALCODESQL
{
  SRVC.MANDT,
  WAPC.WORKAREA as WORK_AREA,
  SRVC.SERVICEUUID as SERVICE_UUID
}
where
  SRVC.MANDT = $parameters.P_Client;
