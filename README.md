@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Aggregated Service Durations per Tour'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #L,
  dataClass: #MIXED
}
define view entity ZI_TOUR_SVC_SUM_WR
  as select from /PLCE/C_PDMNLTourServiceAsgWR as asg
    inner join   /PLCE/C_PDMNLServiceWR        as svc on asg.ServiceUUID = svc.ServiceUUID
{
  key asg.TourUUID,
      
      // Summing up the fields directly in the database
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      sum(svc.TotalDuration)     as TotalServiceDuration,
      
      sum(svc.zz_timeadjustment) as TotalTimeAdjustment,
      
      // Required for quantity fields in aggregated views
      svc.TotalDurationUnit
}
group by
  asg.TourUUID,
  svc.TotalDurationUnit
