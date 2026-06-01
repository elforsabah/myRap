managed implementation in class /plce/bp_r_pdservice unique;
strict ( 2 );
extensible
{
  with determinations on modify;
  with validations on save;
}
with draft;

define behavior for /PLCE/R_PDService alias Service
persistent table /plce/tpdsrv
draft table /plce/dpdsrv
lock master total etag LastChangedAt
authorization master ( global )
etag master LocalLastChangedAt
extensible
{
  create;
  update;
  delete ( features : instance );

  field ( readonly, numbering : managed ) ServiceUUID;

  association _ServiceTask { create ( features : instance ); with draft; }
  association _Attachments { create; with draft; }
  association _Notes { create; with draft; }
  association _StatusHistory { create; with draft; }

  association _ExtUniversal { create; with draft; }
  association _ExtWaste { create; with draft; }
  association _ExtCustom { create; with draft; }

  action ( features : instance, precheck ) assignTour parameter /PLCE/D_PDTourP result [0..*] $self;
  //action ( features : instance, precheck ) assignTour parameter /PLCE/D_PDTourP result [1] $self; //error in invoke action edit flow (accepts only 1 result)
  action ( features : instance, precheck ) createTour parameter /PLCE/D_PDTourCreate result [0..*] $self; ///PLCE/D_PDTourCreate ///PLCE/D_PDTourTemplateP
  //action ( features : instance ) planAutomatically parameter /PLCE/D_PDSPlanAutomaticallyP; ///PLCE/D_PDPlanAutomaticallyP;

  action ( features : instance ) completeService;
  action ( features : instance ) confirmService;
//  action ( features : instance ) TransferSequence;

  // internal action - trigger async Complete (no pending deviations / completed workstatus)
  action ( lock : none ) TryCompleteService;

  validation validateTasks on save { create; }

  determination calc_changed_stamp on save { create; update; delete; }

  draft action ( features : instance ) Edit;
  draft action Activate optimized;
  draft action Discard;
  draft action Resume;

  draft determine action Prepare extensible
  {
    validation Attachments~validateAttachment;
    //validation ServiceTask~validateServiceTasks;
    validation validateTasks;
  }

  determination calculateServiceId on modify { create; }
  determination setInitialStatus on modify { create; }
  determination UnassignOnDelete on modify { delete; }


  mapping for /PLCE/TPDSRV corresponding
  {
    ServiceUUID = service_uuid;
    ServiceId = service_id;
    Profile = profile;
    Action = action;
    ServiceType = service_type;
    ServiceStatus = service_status;
    ReferenceId = reference_id;
    ReferenceInternalId = reference_int_id;
    ServicePriority = service_priority;
    PlanningStatus = planning_status;
    RequestedDate = requested_date;
    EarliestDate = earliest_date;
    LatestDate = latest_date;
    CustomerInfo = customer_info;
    AdditionalText = additional_text;
    FunctionalLocation = functional_location;
    PlanningPlant = planning_plant;
    ServiceWindow = service_window;
    ServiceWindowStartTime = service_window_start;
    ServiceWindowEndTime = service_window_end;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

define behavior for /PLCE/R_PDServiceTask alias ServiceTask
persistent table /plce/tpdsrvtsk
draft table /plce/dpdsrvtsk
lock dependent by _Service
authorization dependent by _Service
extensible
etag master LocalLastChangedAt
{
  update;
  delete ( features : instance );

  field ( readonly, numbering : managed ) ServiceTaskUUID;
  field ( readonly ) ServiceUUID;
  field ( readonly : update ) SequenceNumber;

  association _Service { with draft; }
  association _ExtUniversal { create; with draft; }
  association _ExtWaste { create; with draft; }
  // association _ExtCustom { create; with draft; }

  // remove Tour Task Assignment
  determination DeactivateOnDelete on modify { delete; }

  // recalculate planning status in service & service task
  determination CalculatePlanningStatus on modify { field TourUUID; }
  determination calculateServiceTaskId on modify { create; }

  // TODO review

  //determination calculateServiceStatus on save { create; update; }
  //determination setStatusCreated on save { create; }

  determination calculateDuration on modify { create; field FunctionalLocationCategory; }

  //determination ClearGeoRoute on save { create; delete; field SequenceNumber; }
  determination ClearGeoRoute on modify { create; delete; field SequenceNumber; }

  validation validateServiceTasks on save ##NOT_ASSIGNED_TO_DETACT { delete; }

  mapping for /PLCE/TPDSRVTSK corresponding
  {
    ServiceTaskUUID = service_task_uuid;
    ServiceUUID = service_uuid;
    TourUUID = tour_uuid;
    ServiceTaskId = service_task_id;
    TaskType = task_type;
    SequenceNumber = sequence_number;
    Duration = duration;
    DurationUnit = duration_unit;
    FunctionalLocationCategory = funcloc_Md_category;
    FunctionalLocation = functional_location;
    PlanningStatus = planning_status;
    IsServiceTaskOptional = is_srvc_tsk_optional;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}


define behavior for /PLCE/R_PDServiceAttachment alias Attachments
persistent table /plce/tpdsrvattm
draft table /plce/dpdsrvattm
lock dependent by _Service
authorization dependent by _Service
etag master LocalLastChangedAt
{
  field ( readonly ) ServiceUUID;
  field ( readonly, numbering : managed ) AttachmentUUID;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;
 // field ( mandatory ) Attachment;

  update;
  delete;
  association _Service { with draft; }

  validation validateAttachment on save { create; field Attachment; }

  mapping for /plce/tpdsrvattm corresponding
  {
    ServiceUUID = service_uuid;
    AttachmentUUID = attachment_uuid;
    Comments = comments;
    Attachment = attachment;
    MimeType = mimetype;
    Filename = filename;
    CreatedByUpload = created_by_upload;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

// Extension Universal

define behavior for /PLCE/R_PDServiceExtUNI alias ExtUniversal implementation in class /plce/bp_r_pdserviceuni unique
persistent table /plce/tpdsrvuni
draft table /plce/dpdsrvuni
lock dependent by _Service
authorization dependent by _Service
{
  field ( readonly ) ServiceUUID;

  update;
  delete;

  association _Service { with draft; }

  mapping for /plce/tpdsrvuni corresponding
  {
    ServiceUUID = service_uuid;
    Product = product;
    ProductQuantity = product_quantity;
    ProductQuantityUnit = product_quantity_unit;
    Packaging = packaging;
    PackagingCount = packaging_quantity;
  }
}


define behavior for /PLCE/R_PDServiceTaskExtUNI alias ExtTaskUniversal implementation in class /plce/bp_r_pdserviceuni unique
persistent table /plce/tpdstskuni
draft table /plce/dpdstskuni
lock dependent by _Service
authorization dependent by _Service
{
  field ( readonly ) ServiceUUID, ServiceTaskUUID;

  update;
  delete;

  association _ServiceTask { with draft; }
  association _Service { with draft; }

  mapping for /plce/tpdstskuni corresponding
  {
    ServiceTaskUUID = service_task_uuid;
    Product = product;
    ProductQuantity = product_quantity;
    ProductQuantityUnit = product_quantity_unit;
    Packaging = packaging;
    PackagingCount = packaging_quantity;
  }
}

// Extension Waste

define behavior for /PLCE/R_PDServiceExtWR alias ExtWaste implementation in class /plce/bp_r_pdservicewr unique
persistent table /plce/tpdsrvwr
draft table /plce/dpdsrvwr
lock dependent by _Service
authorization dependent by _Service
{
  field ( readonly ) ServiceUUID;

  update;
  delete;

  association _Service { with draft; }

//  action ( features : instance ) transfer_sequence ;

  determination UpdatePlantLocation on save { field PlantLocation; }

  mapping for /plce/tpdsrvwr corresponding
  {
    ServiceUUID = service_uuid;
    Material = material;
    MaterialWeight = material_weight;
    MaterialWeightUnit = material_weight_unit;
    PlantLocation = plant_location;
    ContainerSourceLocation = container_source_location;
    ContainerFinalLocation = container_final_location;
    ContainerTypeAtLocation = containertype_atloc;
    ContainerAtLocationCount = container_atloc_count;
    ContainerAtLocationTidnr = container_atloc_tidnr;
    ContainerTypeNew = containertype_new;
    ContainerNewCount = container_new_count;
    ContainerNewTidnr = container_new_tidnr;
    EwcCode = ewc_code;
    ServiceFrequency = service_freq;
  }
}

define behavior for /PLCE/R_PDServiceTaskExtWR alias ExtTaskWaste implementation in class /plce/bp_r_pdservicewr unique
persistent table /plce/tpdstskwr
draft table /plce/dpdstskwr
lock dependent by _Service
authorization dependent by _Service
{
  field ( readonly ) ServiceUUID, ServiceTaskUUID;

  update;
  delete;

  association _ServiceTask { with draft; }
  association _Service { with draft; }

  mapping for /plce/tpdstskwr corresponding
  {
    ServiceTaskUUID = service_task_uuid;
    Material = material;
    MaterialWeight = material_weight;
    MaterialWeightUnit = material_weight_unit;
    ContainerType = containertype;
    ContainerCount = container_count;
    ContainerTidnr = container_tidnr;
  }
}

// Extension Custom
define behavior for /PLCE/R_PDServiceExtCustom alias ExtCustom
persistent table /plce/tpdsrvcst
draft table /plce/dpdsrvcst
lock dependent by _Service
authorization dependent by _Service
extensible
{
  field ( readonly ) ServiceUUID;

  update;
  delete;

  association _Service { with draft; }

  mapping for /plce/tpdsrvcst corresponding extensible
  {
    ServiceUUID = service_uuid;
  }
}
//define behavior for /PLCE/R_PDServiceTaskExtCustom alias ExtTaskCustom
//persistent table /plce/tpdsrvtcst
//draft table /plce/dpdsrvtcst
//lock dependent by _Service
//authorization dependent by _Service
//extensible
//{
//  field ( readonly ) ServiceUUID, ServiceTaskUUID;
//
//  update;
//  delete;
//
//  association _ServiceTask { with draft; }
//  association _Service { with draft; }
//
//  mapping for /plce/tpdsrvtcst corresponding extensible
//  {
//    ServiceTaskUUID = service_task_uuid;
//  }
//}


define behavior for /PLCE/R_PDServiceNote alias Notes
persistent table /plce/tpdsrvnote
draft table /plce/dpdsrvnote
lock dependent by _Service
authorization dependent by _Service
etag master LocalLastChangedAt
{
  field ( readonly ) ServiceUUID;
  field ( readonly, numbering : managed ) ServiceNoteUUID;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;

  update;
  delete;
  association _Service { with draft; }

//  validation validateNote on save { create; field ServiceNote; }

  mapping for /plce/tpdsrvnote corresponding
  {
    ServiceUUID = service_uuid;
    ServiceNoteUUID = service_note_uuid;
    ServiceNote = service_note;
    ServiceNoteDesc = service_note_text;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

define behavior for /PLCE/R_PDServiceStatusHistory alias StatusHistory
persistent table /plce/tpdsrvsth
draft table /plce/dpdsrvsth
lock dependent by _Service
authorization dependent by _Service
//etag master LocalLastChangedAt
{
  field ( readonly ) ServiceUUID;
  field ( readonly, numbering : managed ) UUID;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;

  update;
  delete;
  association _Service { with draft; }

  mapping for /plce/tpdsrvsth corresponding
  {
    ServiceUUID = service_uuid;
    UUID = uuid;
    ServiceStatus = service_status;
    PlanningStatus = planning_status;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}


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
  association [0..*] to /PLCE/P_PDCFDeviation_L        as _ServiceDeviation   on _ServiceDeviation.ServiceUUID = $projection.ServiceUUID
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
      @EndUserText.label: 'Planning Plant'
      planning_plant        as PlanningPlant,

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
      _ServiceDeviation,
      _Notes,
      _StatusHistory,

      _ExtUniversal,
      _ExtWaste,
      _ExtCustom

}



@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning and Dispatching Tour'
@ObjectModel.semanticKey: ['TourId']
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define root view entity /PLCE/R_PDTour
  as select from /plce/tpdtour
  // compositions
  composition [0..*] of /PLCE/R_PDTourTaskAssignment as _TaskAssignments
  composition [0..*] of /PLCE/R_PDTourServiceAsgmt   as _ServiceAssignments
  composition [0..*] of /PLCE/R_PDTourResource       as _Resources
  composition [0..*] of /PLCE/R_PDTourAttachment     as _Attachments
  composition [0..*] of /PLCE/R_PDTourTask           as _TourTasks
  composition [0..*] of /PLCE/R_PDTourProductGroup   as _ProductGroups
  composition [0..*] of /PLCE/R_PDTourFuncLoc        as _FunctionalLocations
  composition [0..*] of /PLCE/R_PDTourPlant          as _Plants
  composition [0..*] of /PLCE/R_PDTourProfile        as _Profiles
  composition [0..1] of /PLCE/R_PDTourGeoRoute       as _GeoRoutes
  composition [0..*] of /PLCE/R_PDTourStatusHistory  as _StatusHistory
  //extensions  
  composition [0..1] of /PLCE/R_PDTourExtCustom      as _ExtCustom

  // associations
  //  association [1]    to /PLCE/R_PDProfile               as _Profile                on  $projection.PDProfile = _Profile.Profile
  association [1]    to /PLCE/R_PDTourTemplate       as _TourTemplate              on  $projection.TourTemplate = _TourTemplate.TourTemplate
  association [0..1] to /PLCE/R_PDFunctionalLocation as _PDStartFunctionalLocation on  $projection.StartFunctionalLocation = _PDStartFunctionalLocation.FunctionalLocation
  association [0..1] to /PLCE/R_PDFunctionalLocation as _PDEndFunctionalLocation   on  $projection.EndFunctionalLocation = _PDEndFunctionalLocation.FunctionalLocation
  association [0..*] to /PLCE/P_PDTourStatusT        as _PDTourStatusText          on  $projection.TourStatus = _PDTourStatusText.Value
  association [0..1] to /PLCE/R_PDTourResource       as _MainResource              on  $projection.MainResourceId = _MainResource.ResourceId
                                                                                   and $projection.TourUUID       = _MainResource.TourUUID
  association [1..1] to /PLCE/R_PDTour_L             as _TourLookup                on  $projection.TourUUID = _TourLookup.TourUUID
  //TODO anpassen nach AssignmentUmbau
  association [1]    to /PLCE/P_PDTourStatistic      as _Statistic                 on  _Statistic.TourUuid = $projection.TourUUID
  association [0..*] to /PLCE/P_PDWorkAreaTour       as _WorkAreaTours             on  _WorkAreaTours.TourUUID = $projection.TourUUID

  association [0..*] to /PLCE/R_PDMTourServiceResult as _TourResults               on  $projection.TourUUID = _TourResults.TourUUID

  association [0..*] to /PLCE/R_BGProcessingEntity   as _PendingProcessings        on  _PendingProcessings.EntityType = 'TOUR'
                                                                                   and _PendingProcessings.EntityUUID = $projection.TourUUID
{
      @Semantics.uuid: true
      @UI.hidden: true
  key tour_uuid                      as TourUUID,
      @EndUserText.label: 'Tour ID'
      tour_id                        as TourId,
      @ObjectModel.foreignKey.association: '_TourTemplate'
      tour_template                  as TourTemplate,
      //      tour_type                  as TourType,
//      @ObjectModel.text.association: '_PDTourStatusText'
      tour_status                    as TourStatus,

      @EndUserText.label: 'Scheduled Start'
      @Semantics.dateTime: true
      date_time_start                as ScheduledDateTimeStart, //TODO default calculation / later optimizer
      @EndUserText.label: 'Tour Start Date'
      date_start                     as StartDate,
      @EndUserText.label: 'Tour End Date'
      date_end                       as EndDate,
      earliest_start_time            as EarliestStartTime,
      latest_start_time              as LatestStartTime,
      start_functional_location      as StartFunctionalLocation,
      end_functional_location        as EndFunctionalLocation,
      @EndUserText.label: 'Maximum Tour Duration'
      @Semantics.quantity.unitOfMeasure: 'MaximumTourDurationUnit'
      maximum_tour_duration          as MaximumTourDuration,
      maximum_tour_duration_unit     as MaximumTourDurationUnit,

      @Semantics.quantity.unitOfMeasure: 'CalculatedTourDurationUnit'
      calculated_tour_duration       as CalculatedTourDuration,
      calculated_tour_duration_unit  as CalculatedTourDurationUnit,
      @EndUserText.label: 'Capacity used (in %)'
      calculated_tour_capacity       as CalculatedTourCapacity,

      external_tour_id               as ExternalTourID,
      created_for_simulation         as CreatedForSimulation,
      //      pd_profile                                           as PDProfile,

      @EndUserText.label: 'Main Resource'
      main_resource_id               as MainResourceId,

      @EndUserText.label: 'Additional Information'
      additional_text                as AdditionalText,

      @EndUserText.label: 'Resource Assignment Changed At'
      resource_assignment_changed_at as ResourceAssignmentChangedAt,
      // is updated when a /PLCE/R_PDTourTaskAssignment was changed 
      @EndUserText.label: 'Service Assignment Changed At'
      service_assignments_changed_at as ServiceAssignmentChangedAt,

      @EndUserText.label: 'Sequence Mode'
      sequence_mode                  as SequenceMode,


      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by                     as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at                     as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by                as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at                as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at          as LocalLastChangedAt,

      //only for controlling process flow / no persistence
      cast('' as /plce/char20 )      as SessionProcessFlag,

      //Compositions
      _Resources,
      _Attachments,

      //Association
      _Statistic,
      _MainResource,
      _TourTasks,
      _TourTemplate,
      _TourLookup,
      _ProductGroups,
      _FunctionalLocations,
      _Plants,
      _PDStartFunctionalLocation,
      _PDEndFunctionalLocation,
      _PDTourStatusText,
      //      _Profile,
      _TaskAssignments,
      _ServiceAssignments,
      _WorkAreaTours,
      _Profiles,
      _TourResults,
      _GeoRoutes,
      _StatusHistory,
      _PendingProcessings,
      
      //Extension
      _ExtCustom
}


@Metadata.layer: #PARTNER

annotate entity /PLCE/C_PDMNLTourWR
    with 
{
    @UI.lineItem: [{ position: 1 },
                 { hidden: true },
                { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Spur ermitteln', invocationGrouping: #CHANGE_SET, position: 40  },
                 { type:#FOR_ACTION, dataAction:'releaseTour', label:'Tour Freigeben', invocationGrouping: #CHANGE_SET, position: 30  },
                 { type:#FOR_ACTION, dataAction:'recallTour', label:'Tour zurück rufen', invocationGrouping: #CHANGE_SET, position: 25 },
                 { type:#FOR_ACTION, dataAction:'createtour', label:'Tour anlegen', invocationGrouping: #CHANGE_SET, position: 10 },
                 { type:#FOR_ACTION, dataAction:'touranBMSfreigeben', label:'Tour an BMS freigeben', invocationGrouping: #CHANGE_SET, position: 11 },
                 { type:#FOR_ACTION, dataAction:'deleteTour', label:'Tour Löschen', invocationGrouping: #CHANGE_SET, position: 20 },
                 { type:#FOR_ACTION, dataAction:'SequenceAutomated', label: 'Tour automatisch ordnen', position: 50 },
                 { type:#FOR_ACTION, dataAction:'TransferSequence', label:'Servicereihfolge übertragen', invocationGrouping: #CHANGE_SET, position: 70 },
                 { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Spur ermitteln', invocationGrouping: #CHANGE_SET, qualifier: 'DefaultMap' }
                        
               ]                             
//   
   @UI.hidden: true
    TourUUID; 
 
    // Remaining Duration (e.g. 280)
  @EndUserText.label: 'Restdauer(freie Kapazität)'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]  
  zz_remaining_duration;
 
  // Capacity Used (e.g. 230)
  @EndUserText.label: 'Kapazitätsauslastung(in Min)'
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Kapazitätsauslastung(in Min)', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Kapazitätsauslastung(in Min)', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint: {
    visualization: #PROGRESS,
    targetValueElement: 'MaximumTourDuration_Converted', 
//    criticality: 'TourCapacityColorValue'
    criticality: 'zz_capacity_criticality'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;
  
  @UI.hidden: true
  tour_duration_min;
  
  @UI.hidden: true
  @UI.lineItem: [{ position: 450, importance: #HIGH }] 
  TourDuration;

  @UI.hidden: true
  MaximumTourDuration_Converted;
  
  // 1. Hide the standard SAP field (Hours)
//  @UI.hidden: true
  @UI.lineItem: [{ position: 121, importance: #HIGH }]
  DrivingTime;

  // 2. Show your new custom field (Minutes)
  @UI.lineItem: [{ position: 122, importance: #HIGH }]
  @EndUserText.label: 'Fahrzeit'  // Driving Time
  zz_driving_time_min;
  
  
//  @UI.lineItem: [{ position: 11, importance: #HIGH }] 
// zz_bms_status;
}



(NEU) Ist es möglich, nach dem Update der Entsorgungsanlage noch folgendes einzufügen?
Falls Service auf einer Tour verplant, dann automatisch Spur neu ermitteln (calculate track).
