@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Aggregated Service Durations per Tour'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #S,
  dataClass: #MIXED
}
define view entity ZI_TOUR_SVC_SUM_WR
  as select from /PLCE/R_PDTourServiceAsgmt as asg
    inner join   /PLCE/R_PDService        as svc  on asg.ServiceUUID = svc.ServiceUUID
    // Join our new pre-aggregated Task view
    left outer join ZI_TOUR_TASK_SUM_WR   as task on asg.TourUUID = task.TourUUID
{
  key asg.TourUUID,
      
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      sum(svc._Statistic.TotalDuration) as TotalServiceDuration,
      
      // Keep your time adjustment sum
      sum(svc._ExtCustom.zz_timeadjustment) as TotalTimeAdjustment,

      // Expose the Task Duration (using MAX because it's already aggregated per TourUUID)
      @Semantics.quantity.unitOfMeasure: 'TaskDurationUnit'
      max(task.TotalTaskDuration) as TotalTaskDuration,
      
      svc._Statistic.TotalDurationUnit,
      task.DurationUnit as TaskDurationUnit
}
group by
  asg.TourUUID,
  svc._Statistic.TotalDurationUnit,
  task.DurationUnit
