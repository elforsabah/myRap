@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'P&D Service Statistics'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDServiceStatistic
  as select from /PLCE/R_PDServiceTask
{
  key ServiceUUID,
      cast( 'H' as abap.unit(3) )                                                                                                                                                          as TotalDurationUnit,
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      @EndUserText.label: 'Total Duration'
      cast( sum( unit_conversion( quantity => Duration, source_unit => DurationUnit, target_unit => $projection.TotalDurationUnit, error_handling => 'SET_TO_NULL' ) ) as /plce/duration ) as TotalDuration,
      count( distinct ServiceTaskUUID )                                                                                                                                                    as TaskCount,
      sum( case when TourUUID = hextobin('00000000000000000000000000000000') then 1 else 0 end )                                                                                           as PlannedTasks,
      sum( case when TourUUID = hextobin('00000000000000000000000000000000') then 0 else 1 end )                                                                                           as UnplannedTasks
}
group by
  ServiceUUID
