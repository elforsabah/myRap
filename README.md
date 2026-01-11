managed implementation in class /plce/bp_r_pdtour unique;
strict ( 2 );
extensible
{
  with determinations on modify;
  with validations on save;
}
with draft;

define behavior for /PLCE/R_PDTour alias Tour
persistent table /plce/tpdtour ##UNMAPPED_FIELD
draft table /plce/dpdtour
lock master total etag LastChangedAt
authorization master ( global ) //instance
etag master LocalLastChangedAt
extensible
{
  field ( readonly, numbering : managed ) TourUUID;
  field ( readonly : update ) TourId;
  field ( mandatory : create, readonly : update ) TourTemplate;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;

  association _TaskAssignments { internal create; with draft; }
  association _ServiceAssignments { internal create; with draft; }
  association _TourTasks { create; with draft; }
  association _ProductGroups { create; with draft; }
  association _Resources { create; with draft; }
  association _FunctionalLocations { create; with draft; }
  association _Plants { create; with draft; }
  association _Attachments { create; with draft; }
  association _Profiles { create; with draft; }
  association _GeoRoutes { create; with draft; }
  association _StatusHistory { create; with draft; }
  association _ExtCustom { create; with draft; }

  group Default implementation in class /plce/bp_r_pdtour unique
  {
    create;
    update;
    delete;

    draft action ( features : instance ) Edit;
    draft action Activate; //action definition as optimized is suggested, but it is uncertain which determination and validation executions are skipped
    draft action Discard;
    draft action Resume;

    action AddResource parameter /PLCE/D_PDTourResource result [1] $self;
    action AddAttachment parameter /PLCE/D_PDTourAttachment result [1] $self;

    //    validation validateRequiredFields on save { field PDProfile, TourTemplate; create; update; }
    validation validateRequiredFields on save { field TourTemplate; create; update; }

    // create new tour from template
    static factory action ( precheck ) createTourWithTemplate parameter /PLCE/D_PDTourCreate [1];
    determination FillTemplateData on modify { field TourTemplate; create; }


    // re-calculate tour duration
    action calculateDuration;
    determination AdjustTourDuration on save { field MaximumTourDuration, MaximumTourDurationUnit; }
    determination AdjustScheduledStart on modify { field StartDate, EarliestStartTime; }

    // remove existing tour procs
    determination DeleteTourProcessings on save { delete; }
    // after tour removal, no tour proces must remain
    validation CheckTourProcessings on save ##NOT_ASSIGNED_TO_DETACT { delete; }

    action ( features : instance, precheck ) releaseTour result [1] $self;
    action ( features : instance, precheck ) recallTour result [1] $self;
    action ( features : instance, precheck ) deleteTour result [1] $self;
    action ( features : instance ) completeTour;
    action ( features : instance ) confirmTour;

    // internal action - trigger async Complete (no pending deviations / completed workstatus)
    action ( lock : none  ) TryCompleteTour;

    // direct create without ObjectPage
    action createTourAttachment result [1] $self;//entity /PLCE/R_PDTourAttachment;

    action ( features : instance ) TransferSequence;

    determination setInitialStatus on modify { create; }


    determination calculateTourId on modify { create; }
    draft determine action Prepare extensible
    {
      validation validateRequiredFields;
//      validation Attachments~validateAttachment;
    }

    action ( features : instance, precheck ) GenerateGeoRoute;
    action ( features : instance ) DeleteGeoRoute result [1] $self;


    action SequenceAutomated parameter /PLCE/D_PDTourWorkArea result [1] $self; // parameter /PLCE/D_PDTourSequenceAutomated parameter /PLCE/D_PDTourWorkArea

    //determination ClearGeoRoute on save { field StartFunctionalLocation, EndFunctionalLocation; }
    determination ClearGeoRoute on modify { field StartFunctionalLocation, EndFunctionalLocation; }

  }



  group Assignment implementation in class /plce/bp_r_pdtour_assignment unique
  {
    action AssignTasks deep parameter /PLCE/D_PDAssignTasks;
    action ( precheck ) AssignServices deep parameter /PLCE/D_PDServiceP;   // later (with friends) internal, gets called by Service
    action ( precheck ) ResequenceAllServices deep parameter /PLCE/D_PDTourMoveServiceP;    // same, gets called by PDMTour

    internal action SequenceTasks deep parameter /PLCE/D_PDAssignTasks;
    internal action ResequenceServiceTasksOfTour deep parameter /PLCE/D_PDResequenceTasksP;
    internal action UnassignServiceTasks deep parameter /PLCE/D_PDServiceTaskP;
    internal action SetSequenceMode parameter /plce/pdtoursequencemode;
  }

  mapping for /PLCE/TPDTOUR corresponding
  {
    TourId = tour_id;
    TourUUID = tour_uuid;
    TourTemplate = tour_template;
    TourStatus = tour_status;
    StartDate = date_start;
    EndDate = date_end;
    ScheduledDateTimeStart = date_time_start;
    EarliestStartTime = earliest_start_time;
    LatestStartTime = latest_start_time;
    StartFunctionalLocation = start_functional_location;
    EndFunctionalLocation = end_functional_location;
    MaximumTourDuration = maximum_tour_duration;
    MaximumTourDurationUnit = maximum_tour_duration_unit;
    CalculatedTourDuration = calculated_tour_duration;
    CalculatedTourDurationUnit = calculated_tour_duration_unit;
    CalculatedTourCapacity = calculated_tour_capacity;
    ExternalTourID = external_tour_id;
    CreatedForSimulation = created_for_simulation;
    //    PDProfile = pd_profile;
    MainResourceId = main_resource_id;
    SequenceMode = sequence_mode;
    ResourceAssignmentChangedAt = resource_assignment_changed_at;
    ServiceAssignmentChangedAt = service_assignments_changed_at;
    AdditionalText = additional_text;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }

  //mapping for moving data tour template -> tour
  mapping for /plce/cpdttmpl
  {
    EarliestStartTime = earliest_start_time;
    LatestStartTime = latest_start_time;
    StartFunctionalLocation = start_functional_location;
    EndFunctionalLocation = end_functional_location;
    MaximumTourDuration = maximum_tour_duration;
    MaximumTourDurationUnit = maximum_tour_duration_unit;
    //    PDProfile = pd_profile;

    //TODO Felder werden durch Simulation bestueckt, offen: Wie wird Tour
    //    WaitingTime = waiting_time;
    //    WaitingTimeUnit = waiting_time_unit;
    //    DrivingTime = driving_time;
    //    DrivingTimeUnit = driving_time_unit;


  }
}


// Tour - Service Assignment
define behavior for /PLCE/R_PDTourServiceAsgmt alias ServiceAssignment implementation in class /plce/bp_r_pdtour_assignment unique
persistent table /plce/tpdtassrvc
draft table /plce/dpdtassrvc ##UNMAPPED_FIELD
lock dependent by _Tour
authorization dependent by _Tour
{
  internal update;
  internal delete;

  field ( readonly ) TourUUID;
  field ( mandatory : create, readonly : update ) ServiceUUID;
  field ( readonly ) TourSequence;

  association _Tour { with draft; }
  association _Service { }
  association _ServiceTasksAssignments { with draft; }

  action ( features : instance ) UnAssign;
  action ReplanService;
  action ( features : instance ) MoveService parameter /PLCE/D_PDServiceMove result [1..*] $self;

  //determination calc_changed_stamp on save { create; update; delete; }
  determination assignment_changed on save { field Removed, TourSequence ; create; update; delete; }

  mapping for /plce/tpdtassrvc corresponding
  {
    TourUUID = tour_uuid;
    ServiceUUID = service_uuid;
    LeadServiceTaskUUID = lead_service_task_uuid;
    Removed = removed;

    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

//Tour - Task Assignment
define behavior for /PLCE/R_PDTourTaskAssignment alias TaskAssignment implementation in class /plce/bp_r_pdtour_assignment unique
persistent table /plce/tpdtasta
draft table /plce/dpdtasta
lock dependent by _Tour
authorization dependent by _Tour
{
  internal update;
  internal delete;

  field ( readonly ) TourUUID;
  field ( mandatory : create, readonly : update ) TaskScope, ServiceTaskUUID, ServiceUUID, TourTaskUUID;

  association _Tour { with draft; }
  association _Service { }

  action Deactivate;

  mapping for /plce/tpdtasta corresponding
  {
    ServiceTaskUUID = service_task_uuid;
    ServiceUUID = service_uuid;
    TaskScope = task_type;
    TourTaskUUID = tour_task_uuid;
    TourUUID = tour_uuid;
    TourSequence = sequence_number;
    DateTimeStart = date_time_start;
    Duration = duration;
    DurationUnit = duration_unit;
    Removed = removed;

  }
  determination calc_changed_stamp on save { create; update; delete; }

  determination UnassignOnDelete on modify { delete; }

  //  determination AdjustServiceAssignmentsC on modify { create; }
  //  determination AdjustServiceAssignmentsD on modify { delete; }
  determination AdjustServiceAssignments on modify { create; delete; field Removed; }
  determination AdjustTourDuration on save { create; update; delete; field Duration, DurationUnit; }

  determination ClearGeoRoute on save { create; field TourSequence, Removed; }
}


define behavior for /PLCE/R_PDTourTask alias TourTask
persistent table /plce/tpdtourtsk
draft table /plce/dpdtourtsk
lock dependent by _Tour
authorization dependent by _Tour
etag master LocalLastChangedAt
{
  update;
  delete;

  field ( readonly ) TourUUID;
  //field ( readonly, numbering : managed ) TourTaskUUID;
  field ( readonly : update, numbering : managed ) TourTaskUUID;

  association _Tour { with draft; }

  determination CreateTourTaskAssignment on modify { create; }
  determination RemoveTourTaskAssignment on modify { delete; }

  determination SequenceTourTask on save { create; delete; update; }

  mapping for /PLCE/TPDTOURTSK corresponding
  {
    TourTaskUUID = tour_task_uuid;
    TaskCategory = task_category;
    TaskType = task_type;
    TourTaskID = tour_task_id;
    TourTaskStatus = tour_task_status;
    IsCalculated = is_calculated;
    TourUUID = tour_uuid;
    Duration = tour_task_duration;
    DurationUnit = tour_task_duration_unit;
    EarliestStart = earliest_start;
    EarliestStartUnit = earliest_start_unit;
    LatestStart = latest_start;
    LatestStartUnit = latest_start_unit;

    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}



define behavior for /PLCE/R_PDTourResource alias Resource implementation in class /plce/bp_r_pdtour_resource unique
persistent table /plce/tpdtrres
draft table /plce/dpdtrres
lock dependent by _Tour
authorization dependent by _Tour
etag master LocalLastChangedAt
{
  field ( readonly ) TourUUID;
  field ( mandatory:create, readonly:update ) ResourceId;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;
  field ( suppress ) CommunicationUser;
  association _Tour { with draft; }

  update;
  delete;

  action deleteResource result [0..*] $self;
  action ( precheck ) changeMainResource result [1..*] $self;

  determination calc_changed_stamp on save { create; update; delete; } //TODO ggf verlagern in MTour
  determination determine_tourmainresource on save { create; update; delete; }

  determination ActivateTourProcessing on save { create; }
  determination DeactivateTourProcessing on save { delete; }

  mapping for /plce/tpdtrres corresponding
  {
    ResourceId = resource_id;
    TourUUID = tour_uuid;
    IsMainResource = is_main_resource;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

define behavior for /PLCE/R_PDTourAttachment alias Attachments
persistent table /plce/tpdtrattm
draft table /plce/dpdtrattm
lock dependent by _Tour
authorization dependent by _Tour
etag master LocalLastChangedAt
{
  field ( readonly ) TourUUID;
  field ( readonly, numbering : managed ) AttachmentUUID;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;
  field ( mandatory ) Attachment;

  //static factory action addAttachment parameter /PLCE/D_PDTourAttachment [1];
  action deleteAttachment result [0..*] $self;

  update;
  delete;
  association _Tour { with draft; }

//  validation validateAttachment on save { create; field Attachment; }

  mapping for /plce/tpdtrattm corresponding
  {
    TourUUID = tour_uuid;
    AttachmentUUID = attachment_uuid;
    Comments = comments;
    Attachment = attachment;
    MimeType = mimetype;
    Filename = filename;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

define behavior for /PLCE/R_PDTourProductGroup alias TourProductGroups
persistent table /plce/tpdtprdg
draft table /plce/dpdtprdg
lock dependent by _PDTour
authorization dependent by _PDTour
{
  field ( readonly ) TourUUID;
  field ( mandatory:create, readonly:update ) ProductGroup;

  update;
  delete;
  association _PDTour { with draft; }
  mapping for /plce/tpdtprdg corresponding
  {
    TourUUID = tour_uuid;
    ProductGroup = product_group;
  }
}

define behavior for /PLCE/R_PDTourFuncLoc alias FunctionalLocations
persistent table /plce/cpdtourfl
draft table /plce/dpdtourfl
lock dependent by _Tour
authorization dependent by _Tour
{
  field ( readonly ) TourUuid;
  field ( mandatory:create, readonly:update ) FunctionalLocation;
  update;
  delete;
  association _Tour { with draft; }
  mapping for /plce/cpdtourfl corresponding
  {
    TourUuid = tour_uuid;
    FunctionalLocation = functional_location;
  }
}

define behavior for /PLCE/R_PDTourPlant alias Plants
persistent table /plce/tpdtplant
draft table /plce/dpdtplant
lock dependent by _Tour
authorization dependent by _Tour
{
  field ( readonly ) TourUUID;
  field ( mandatory:create, readonly:update ) Plant;
  update;
  delete;
  association _Tour { with draft; }
  mapping for /plce/tpdtplant corresponding
  {
    TourUUID = tour_uuid;
    Plant = plant;
  }
}

define behavior for /PLCE/R_PDTourProfile alias Profiles
persistent table /plce/tpdtprof
draft table /plce/dpdtprof
lock dependent by _Tour
authorization dependent by _Tour
{
  field ( readonly ) TourUUID;
  field ( mandatory:create, readonly:update ) Profile;
  update;
  delete;
  association _Tour { with draft; }
  mapping for /PLCE/TPDTPROF corresponding
  {
    TourUUID = tour_uuid;
    Profile = profile;
  }
}

define behavior for /PLCE/R_PDTourGeoRoute alias GeoRoutes
persistent table /plce/tpdtrroute
draft table /plce/dpdtrroute
lock dependent by _Tour
authorization dependent by _Tour
etag master LocalLastChangedAt
{
  field ( readonly ) TourUUID;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;

  update;
  delete;
  association _Tour { with draft; }

  mapping for /plce/tpdtrroute corresponding
  {
    TourUUID = tour_uuid;
    Geometry = geometry;
    Duration = duration;
    DurationUnit = duration_unit;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

define behavior for /PLCE/R_PDTourStatusHistory alias StatusHistory
persistent table /plce/tpdtstath
draft table /plce/dpdtstath
lock dependent by _Tour
authorization dependent by _Tour
//etag master LocalLastChangedAt
{
  field ( readonly ) TourUUID;
  field ( readonly, numbering : managed ) UUID;
  field ( readonly ) CreatedAt, CreatedBy, LastChangedAt, LastChangedBy, LocalLastChangedAt;

  update;
  delete;
  association _Tour { with draft; }

  mapping for /plce/tpdtstath corresponding
  {
    TourUUID = tour_uuid;
    UUID = uuid;
    TourStatus = tour_status;
    CreatedBy = created_by;
    CreatedAt = created_at;
    LastChangedBy = last_changed_by;
    LastChangedAt = last_changed_at;
    LocalLastChangedAt = local_last_changed_at;
  }
}

// Extension Custom
define behavior for /PLCE/R_PDTourExtCustom alias ExtCustom
persistent table /plce/tpdtourcst
draft table /plce/dpdtourcst
lock dependent by _Tour
authorization dependent by _Tour
extensible
{
  field ( readonly ) TourUUID;

  update;
  delete;

  association _Tour { with draft; }

  mapping for /plce/tpdtourcst corresponding extensible
  {
    TourUUID = tour_uuid;
  }
}
