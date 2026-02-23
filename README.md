@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Service Raw Data'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #L,
  dataClass: #MIXED
}
define view entity ZI_TourServiceRawData
  as select from /PLCE/C_PDMNLTourServiceAsgWR as asg
    inner join   /PLCE/C_PDMNLServiceWR        as svc on asg.ServiceUUID = svc.ServiceUUID
{
  key asg.TourUUID,
  key svc.ServiceUUID,
      svc.TotalDuration,
      svc.zz_timeadjustment as zz_timeadjust
}
