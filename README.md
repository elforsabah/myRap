extension implementation in class zbp_zpd_bp_r_pdservice unique;

extend behavior for Service
{
  determination ZZServiceCreateDetermination on modify { create; }

  determination SetPointOfOrigin on modify { create; update; }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
 mapping for /plce/tpdsrvcst  {
    zzpoint_origin_wdoi =  zzpoint_origin_wdoi ;
 }
 field ( suppress ) zzpobjnr;

 field ( readonly  ) zzpoint_origin_wdoi;
}


class lhc_Service definition inheriting from CL_ABAP_BEHAVIOR_HANDLER.
  private section.

    methods ZZServiceCreateDetermination for determine on modify
      importing KEYS for Service~ZZServiceCreateDetermination.

    methods SetPointOfOrigin for determine on modify
      importing KEYS for Service~SetPointOfOrigin.


endclass.

class lhc_Service implementation.

 METHOD setpointoforigin.

 READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
    FIELDS ( referenceid )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
  
    
  
  
  
  
    DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.
    SELECT SINGLE point_of_origin
      FROM zi_wr_ewa_order_object
      WHERE referenceid = @<ls_service>-ReferenceId
      INTO @lv_zzpoint_origin_wdoi.

*    IF sy-subrc = 0 AND lv_zzpoint_origin_wdoi IS NOT INITIAL.
*      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
*        ENTITY Service
*        UPDATE FIELDS ( ZZPOINT_ORIGIN_WDOI )
*        WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
*                        ZZPOINT_ORIGIN_WDOI = lv_zzpoint_origin_wdoi ) ).
*    ENDIF.
  ENDLOOP.

 ENDMETHOD.

  method ZZServiceCreateDetermination.
    data:
      LV_CID            type STRING value '$abap_cid1_',
      LT_SERVICE_CREATE type table for create /PLCE/R_PDService\_ExtCustom.

    loop at KEYS reference into data(LKEY).
      insert initial line into table LT_SERVICE_CREATE assigning field-symbol(<LS_SERVICE_CREATE>) else unassign.
      if <LS_SERVICE_CREATE> is assigned.
        <LS_SERVICE_CREATE>-%TKY = LKEY->%TKY.
        <LS_SERVICE_CREATE>-%TARGET = value #( ( %CID = LV_CID && SY-TABIX ) ).
      endif.
    endloop.


    modify entities of /PLCE/R_PDService in local mode
      entity Service
       create by \_ExtCustom
         auto fill cid set fields with LT_SERVICE_CREATE
       mapped data(LMAPPED)
       failed data(LFAILED)
       reported data(LREPORTED).

    /PLCE/CL_BASE_MISC=>CHECK_RESPONSE( exporting IS_RESPONSE = LREPORTED-%OTHER changing CT_MESSAGES = REPORTED-%OTHER ).
  endmethod.

endclass.


extend view entity /PLCE/C_PDMNLServiceWR with 
{
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
   virtual service_criticality : abap.int1,
    /PLCE/R_PDService._ExtCustom.zzpoint_origin_wdoi as point_of_origin
   // Value of this field(flag) will come from the table EWA_ORDER_OBJECT(field ZZPOINT_ORIGIN_WDOI ). 
                                                   // It defines if a service has been replanned
  
//  Similarly to how tours in the Planning Cockpit get a red bar at the start of the line to
//   indicate that a deviation occurred in the tour, we will implement a bar at the start of the
//    line of the service to indicate that this service has been replanned. This means that we 
//    extend the services to include a flag whether the service was created from a copied WDOI. 
//    A filter option on this flag will be added to the Planning Cockpit.
}



extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on   _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association [1] to /plce/tpdsrv as _tpdsrv  on  _tpdsrv.service_uuid  = $projection.ServiceUUID
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _tpdsrv.zzpoint_origin_wdoi,
  _ZZOrderObject
}


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


Description	Resource	Path	Location	Type
No component exists with the name "ZZPOINT_ORIGIN_WDOI".	ZBP_ZPD_BP_R_PDSERVICE (Local Types)	/.adt/classlib/classes/zbp_zpd_bp_r_pdservice	line 39	ABAP Syntax Check Problem






