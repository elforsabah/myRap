@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning and Dispatching Service'
@ObjectModel.semanticKey: ['ServiceId']
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define root view entity /PLCE/R_PDService
  as select from /plce/tpdsrv

  //base compositions
  composition [0..*] of /PLCE/R_PDServiceTask          as _ServiceTask
  composition [0..*] of /PLCE/R_PDServiceAttachment    as _Attachments
  composition [0..*] of /PLCE/R_PDServiceNote          as _Notes
  composition [0..*] of /PLCE/R_PDServiceStatusHistory as _StatusHistory

  //extensions
  composition [0..1] of /PLCE/R_PDServiceExtUNI        as _ExtUniversal
  composition [0..1] of /PLCE/R_PDServiceExtWR         as _ExtWaste
  composition [0..1] of /PLCE/R_PDServiceExtCustom     as _ExtCustom
  //associations
  association [1..1] to /PLCE/R_PDProfile              as _Profile            on $projection.Profile = _Profile.Profile
  association [1..1] to /PLCE/R_PDAction               as _Action             on $projection.Action = _Action.Action
  association [1..1] to /PLCE/R_PDServiceType          as _ServiceType        on $projection.ServiceType = _ServiceType.ServiceType
  association [1]    to /PLCE/R_PDService_L            as _ServiceLookup      on $projection.ServiceUUID = _ServiceLookup.ServiceUUID
  association [0..*] to /PLCE/P_PDPlanningStatusT      as _PlanningStatusText on $projection.PlanningStatus = _PlanningStatusText.Value
  association [0..*] to /PLCE/P_PDServiceWindowT       as _ServiceWindowText  on $projection.ServiceWindow = _ServiceWindowText.Value
  association [1]    to /PLCE/R_PDServiceStatistic     as _Statistic          on $projection.ServiceUUID = _Statistic.ServiceUUID 
  association [0..1] to /PLCE/R_PDFunctionalLocation   as _FunctionalLocation on $projection.FunctionalLocation = _FunctionalLocation.FunctionalLocation

  association [0..*] to /PLCE/P_PDWorkAreaService      as _WorkAreaServices   on $projection.ServiceUUID = _WorkAreaServices.ServiceUUID
  association [0..*] to /PLCE/P_PDTourAssignment       as _TourAssignments    on $projection.ServiceUUID = _TourAssignments.ServiceUUID

  association [0..*] to /PLCE/R_PDMTourServiceResult   as _ServiceResults     on $projection.ServiceUUID = _ServiceResults.ServiceUUID
  association [0..*] to /PLCE/P_PDServiceStatusT       as _ServiceStatusText  on _ServiceStatusText.Value = $projection.ServiceStatus
{
      @Semantics.uuid: true
  key service_uuid          as ServiceUUID,
      @EndUserText.label: 'Service Id'
      service_id            as ServiceId,
      @ObjectModel.foreignKey.association: '_Profile'
      @EndUserText.label: 'Profile'
      profile               as Profile,
      @ObjectModel.foreignKey.association: '_Action'
      @EndUserText.label: 'Action'
      action                as Action,
      @ObjectModel.foreignKey.association: '_ServiceType'
      @EndUserText.label: 'Service Type'
      service_type          as ServiceType,

      service_status        as ServiceStatus,

      @EndUserText.label: 'Order'
      reference_id          as ReferenceId,
      reference_int_id      as ReferenceInternalId,

      @EndUserText.label: 'Service Priority'
      service_priority      as ServicePriority,
      @EndUserText.label: 'Planning Status'
      planning_status       as PlanningStatus,

      @EndUserText.label: 'Requested Date'
      requested_date        as RequestedDate,
      @EndUserText.label: 'Earliest Date'
      earliest_date         as EarliestDate,
      @EndUserText.label: 'Latest Date'
      latest_date           as LatestDate,

      @EndUserText.label: 'Customer'
      customer_info         as CustomerInfo,
      @EndUserText.label: 'Additional Information'
      additional_text       as AdditionalText,
      //note                         as Note, //todo raus
      @ObjectModel.foreignKey.association: '_FunctionalLocation'
      functional_location   as FunctionalLocation,
      //TODO: Do we need a field for the PDCategory of a FunctionalLocation here as well?
      
      @EndUserText.label: 'Service Window'
      service_window as ServiceWindow, 
      @EndUserText.label: 'Service Window Start Time'
      service_window_start as ServiceWindowStartTime,
      @EndUserText.label: 'Service Window End Time'
      service_window_end as ServiceWindowEndTime,
//      concat_with_space( service_window, concat_with_space( service_window_start, service_window_end, 1 ), 1) as ServiceWindowFull,

//      service_freq          as ServiceFrequency,

      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by            as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at            as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by       as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at       as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at as LocalLastChangedAt,


      //Association
      _ServiceTask,
      _Attachments,
      _Profile,
      _Action,
      _ServiceType,
      _FunctionalLocation,
      _ServiceLookup,
      _PlanningStatusText,
      _Statistic,
      _WorkAreaServices,
      _TourAssignments,
      _ServiceResults,
      _ServiceStatusText,
      _ServiceWindowText,
      _Notes,
      _StatusHistory,

      _ExtUniversal,
      _ExtWaste,
      _ExtCustom

}

@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Result Work Area - Service'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/P_PDWorkAreaService
  as select from /PLCE/P_PDWorkAreaServiceFunc( p_client: $session.client )
{
      @Consumption.valueHelpDefinition: [{
            entity: {
              name    : '/PLCE/C_PDWorkAreaUser_VH',
              element : 'WorkArea'
            },
            label     : 'Work Area',
            useForValidation: true
          }]
  key work_area    as WorkArea,
  key service_uuid as ServiceUUID

}

CLASS lhc_service DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS get_global_features FOR GLOBAL FEATURES
      IMPORTING REQUEST requested_features FOR Service RESULT result.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Service RESULT result.

    METHODS assignworkarea FOR MODIFY
      IMPORTING keys FOR ACTION Service~assignworkarea RESULT result.

    METHODS precheck_assignworkarea FOR PRECHECK
      IMPORTING keys FOR ACTION Service~assignworkarea.

ENDCLASS.

CLASS lhc_service IMPLEMENTATION.

  METHOD get_global_features.
  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD assignworkarea.

  if 1 = 2.


  ENDIF.

  ENDMETHOD.

  METHOD precheck_assignworkarea.
  ENDMETHOD.

ENDCLASS.

*"* use this source file for the definition and implementation of
*"* local helper classes, interface definitions and type
*"* declarations

extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{

mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
 }

field ( readonly : update  ) zz_tech_fachbe;

}


