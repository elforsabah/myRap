@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Aggregated Task Durations per Tour'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #S,
  dataClass: #MIXED
}
define view entity ZI_TOUR_TASK_SUM_WR
  as select from /PLCE/R_PDTourTaskAssignment
{
  key TourUUID,
      
      @Semantics.quantity.unitOfMeasure: 'DurationUnit'
      sum(Duration) as TotalTaskDuration,
      
      DurationUnit
}
group by
  TourUUID,
  DurationUnit
