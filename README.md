@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour - Task Assignment'
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDTourTaskAssignment
  as select from /plce/tpdtasta
  association        to parent /PLCE/R_PDTour          as _Tour        on  $projection.TourUUID = _Tour.TourUUID
  association [0..1] to /PLCE/R_PDServiceTask          as _ServiceTask on  _ServiceTask.ServiceTaskUUID = $projection.ServiceTaskUUID
  association [0..1] to /PLCE/R_PDTourTask             as _TourTask    on  _TourTask.TourTaskUUID = $projection.TourTaskUUID
                                                                       and _TourTask.TourUUID     = $projection.TourUUID
  association [0..1] to /PLCE/R_PDService              as _Service     on  _Service.ServiceUUID   = $projection.ServiceUUID
  association [0..*] to /PLCE/P_PDTaskScopeT           as _TaskScopeText on _TaskScopeText.Value  = $projection.TaskScope
  association [1]    to /PLCE/R_PDTourTaskAssignment_L as _Lookup      on  _Lookup.TourUUID        = $projection.TourUUID
                                                                       and _Lookup.TaskScope       = $projection.TaskScope
                                                                       and _Lookup.ServiceUUID     = $projection.ServiceUUID
                                                                       and _Lookup.ServiceTaskUUID = $projection.ServiceTaskUUID
                                                                       and _Lookup.TourTaskUUID    = $projection.TourTaskUUID

  association [0..1] to /PLCE/R_PDTourTaskAsgmtUnify   as _UnifiedTask on  _UnifiedTask.TourUUID        = $projection.TourUUID
                                                                       and _UnifiedTask.TaskScope       = $projection.TaskScope
                                                                       and _UnifiedTask.ServiceUUID     = $projection.ServiceUUID
                                                                       and _UnifiedTask.ServiceTaskUUID = $projection.ServiceTaskUUID
                                                                       and _UnifiedTask.TourTaskUUID    = $projection.TourTaskUUID
                                                                       
{
      //key tour_assignment_uuid as AssignmentUUID,
  key tour_uuid         as TourUUID,
  key task_type         as TaskScope,
  key service_uuid      as ServiceUUID,
  key service_task_uuid as ServiceTaskUUID,
  key tour_task_uuid    as TourTaskUUID,      
      @EndUserText.label: 'Tour Sequence'
      sequence_number   as TourSequence,
      duration          as Duration,
      duration_unit     as DurationUnit,
      date_time_start   as DateTimeStart,
      removed           as Removed,
      _Tour,
      _ServiceTask,
      _Service,
      _TourTask,
      _UnifiedTask,
      _Lookup,
      _TaskScopeText

}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour - Service Assignment'
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDTourServiceAsgmt
  as select from /plce/tpdtassrvc
  association        to parent /PLCE/R_PDTour        as _Tour                      on  _Tour.TourUUID = $projection.TourUUID
  association [1]    to /PLCE/R_PDService            as _Service                   on  _Service.ServiceUUID = $projection.ServiceUUID
  association [1..1] to /PLCE/R_PDTourServiceAsgmt_L as _Lookup                    on  _Lookup.TourUUID    = $projection.TourUUID
                                                                                   and _Lookup.ServiceUUID = $projection.ServiceUUID
  association [0..*] to /PLCE/R_PDTourTaskAssignment as _ServiceTasksAssignments   on  _ServiceTasksAssignments.TourUUID    = $projection.TourUUID
                                                                                   and _ServiceTasksAssignments.TaskScope   = 'S'
                                                                                   and _ServiceTasksAssignments.ServiceUUID = $projection.ServiceUUID
  //and _ServiceTasksAssignments.Removed is initial
  /*+[hideWarning] { "IDS" : [ "CARDINALITY_CHECK" ]  } */
  association [0..1] to /PLCE/R_PDTourTaskAssignment as _LeadServiceTaskAssignment on  _LeadServiceTaskAssignment.TourUUID        = $projection.TourUUID
                                                                                   and _LeadServiceTaskAssignment.TaskScope       = 'S'
                                                                                   and _LeadServiceTaskAssignment.ServiceUUID     = $projection.ServiceUUID
                                                                                   and _LeadServiceTaskAssignment.ServiceTaskUUID = $projection.LeadServiceTaskUUID
  //and _LeadServiceTaskAssignment.TourTaskUUID = $projection.emptyUUID



{
  key tour_uuid                               as TourUUID,
  key service_uuid                            as ServiceUUID,
      lead_service_task_uuid                  as LeadServiceTaskUUID,
      _LeadServiceTaskAssignment.TourSequence as TourSequence,
      @EndUserText.label: 'Removed'
      removed                                 as Removed,
      _Service.ServiceStatus                  as ServiceStatus,
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by                              as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at                              as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by                         as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at                         as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at                   as LocalLastChangedAt,
      /* Associations */
      _Tour,
      _Lookup,
      _Service,
      _ServiceTasksAssignments,
      _LeadServiceTaskAssignment
}



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
    inner join   /PLCE/R_PDService        as svc on asg.ServiceUUID = svc.ServiceUUID
    
    
    
{
  key asg.TourUUID,
      
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      sum(svc._Statistic.TotalDuration) as TotalServiceDuration,
      
      // Look how clean this is now! Natively sums the pure numbers.
      sum(svc._ExtCustom.zz_timeadjustment) as TotalTimeAdjustment,
      
      svc._Statistic.TotalDurationUnit
}
group by
  asg.TourUUID,
  svc._Statistic.TotalDurationUnit
