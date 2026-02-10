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
  draft action Activate;
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

  validation validateServiceTasks on save { delete; }

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
