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


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning and Dispatching Service Task'
@ObjectModel.semanticKey: ['ServiceTaskId']
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDServiceTask
  as select from /plce/tpdsrvtsk

  association        to parent /PLCE/R_PDService       as _Service            on  $projection.ServiceUUID = _Service.ServiceUUID

  //compositions
  composition [0..1] of /PLCE/R_PDServiceTaskExtUNI    as _ExtUniversal
  composition [0..1] of /PLCE/R_PDServiceTaskExtWR     as _ExtWaste
//  composition [0..1] of /PLCE/R_PDServiceTaskExtCustom as _ExtCustom

  association [0..1] to /PLCE/R_PDTour                 as _Tour               on  $projection.TourUUID = _Tour.TourUUID
  association [1..1] to /PLCE/R_PDTaskType             as _TaskType           on  $projection.TaskType = _TaskType.TaskType
  association [1]    to /PLCE/R_PDServiceTask_L        as _ServiceTaskLookup  on  $projection.ServiceTaskUUID = _ServiceTaskLookup.ServiceTaskUUID
                                                                              and $projection.ServiceUUID     = _ServiceTaskLookup.ServiceUUID
  association [0..1] to /PLCE/R_PDFunctionalLocation   as _FunctionalLocation on  $projection.FunctionalLocation = _FunctionalLocation.FunctionalLocation
  association [0..*] to /PLCE/P_PDPlanningStatusT      as _PlanningStatusText on  $projection.PlanningStatus = _PlanningStatusText.Value

{
      @Semantics.uuid: true
  key service_task_uuid      as ServiceTaskUUID,
      @ObjectModel.foreignKey.association: '_Service'
      service_uuid           as ServiceUUID,
      service_task_id        as ServiceTaskId,
      tour_uuid              as TourUUID,
      @ObjectModel.foreignKey.association: '_TaskType'
      @EndUserText.label: 'Task Type'
      task_type              as TaskType,
      @EndUserText.label: 'Task Sequence'
      sequence_number        as SequenceNumber, //todo pruefen -> umbenennen in TaskSequence
      @Semantics.quantity.unitOfMeasure: 'DurationUnit'
      duration               as Duration, //todo pruefen
      @EndUserText.label: 'Duration Unit'
      duration_unit          as DurationUnit, //todo pruefen

      @EndUserText.label: 'Functional Location Category'
      funcloc_md_category    as FunctionalLocationCategory, //todo pruefen, kann raus wenn FL eindeutige Cat.
      @ObjectModel.foreignKey.association: '_FunctionalLocation'
      functional_location    as FunctionalLocation,
      _Service.ServiceStatus as ServiceStatus,
      planning_status        as PlanningStatus,
      //planning
      is_srvc_tsk_optional   as IsServiceTaskOptional,
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by             as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at             as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by        as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at        as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at  as LocalLastChangedAt,

      //parent association
      _Service,
      //compositions
      _ExtUniversal,
      _ExtWaste,
//      _ExtCustom,

      //Association
      _Tour,
      _TaskType,
      _FunctionalLocation,
      _ServiceTaskLookup,
      _PlanningStatusText
}
