@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Projection View of ZI_PRINTCONFIG'
@Metadata.ignorePropagatedAnnotations: true

@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}

define root view entity ZC_PRINTCONFIG 
provider contract transactional_query
as projection on ZI_PRINTCONFIG

{
   @UI.selectionField: [{ position: 10 }]
   @UI.lineItem: [{ position: 10 }]
    key Field,
    @UI.lineItem: [{ position: 20 }]
    key Printform,
    @UI.lineItem: [{ position: 30 }]
    key Formtype,
    @UI.lineItem: [{ position: 40 }]
    ParameterIn,
    @UI.lineItem: [{ position: 50 }]
    Filenameteemplate
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'P&D Attachments'
@Metadata.ignorePropagatedAnnotations: true
define view entity ZI_PDATTACHMENT
  as select from /plce/tpdtrattm
{

  key tour_uuid       as Uuid,
  key attachment_uuid as AttachmentUuid,
      filename        as Filename,
      mimetype        as Mimetype,
      attachment      as Attachment,
      created_by      as CreatedBy,
      created_at      as CreatedAt
}
union all select from /plce/tpdsrvattm
{
  key service_uuid    as Uuid,
  key attachment_uuid as AttachmentUuid,
      filename        as Filename,
      mimetype        as Mimetype,
      attachment      as Attachment,
      created_by      as CreatedBy,
      created_at      as CreatedAt

}


@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Interface View for ZWR_CWAPRTCONF'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZI_PRINTCONFIG
  as select from zwr_cwaprtconf
{

      @UI.selectionField: [{ position: 10 }]
      @UI.lineItem: [{ position: 10 }]
  key field             as Field,
      @UI.lineItem: [{ position: 20 }]
  key printform         as Printform,
      @UI.lineItem: [{ position: 30 }]
  key formtype          as Formtype,
      @UI.lineItem: [{ position: 40 }]
      parameter_in      as ParameterIn,
      @UI.lineItem: [{ position: 500 }]
      filenameteemplate as Filenameteemplate
}


@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'R View on ZI_PDATTACHMENT'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZR_PDATTACHMENT
  as select from ZI_PDATTACHMENT

  association     to parent ZR_PDTOUR           as _Tour         on $projection.TourUUID = _Tour.TourUuid

  association [1] to /PLCE/R_PDTourServiceAsgmt as _ServiceAsgmt on _ServiceAsgmt.ServiceUUID = $projection.Uuid

{
  key Uuid,
      @Semantics.uuid: true
  key AttachmentUuid,
      @ObjectModel.foreignKey.association: '_Tour'
  key case when _ServiceAsgmt.TourUUID is null then Uuid else _ServiceAsgmt.TourUUID end as TourUUID,
      _ServiceAsgmt.TourSequence,
      _ServiceAsgmt._Service.ReferenceId,
      Filename,
      @Semantics.mimeType: true
      Mimetype,
      @Semantics.largeObject: { mimeType: 'Mimetype',
                                fileName: 'Filename',
                                contentDispositionPreference: #INLINE }
      Attachment,
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      CreatedAt,

      _Tour
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'R View on /PLCE/TPDTOUR'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZR_PDTOUR
  as select from /plce/tpdtour
  // compositions
  composition [0..*] of ZR_PDTOURSERVICEASGMT  as _ServiceAssignments
  composition [0..*] of ZR_PDATTACHMENT        as _Attachments

  // associations
  association [1..1] to /PLCE/R_PDTour_L       as _TourLookup       on $projection.TourUuid = _TourLookup.TourUUID
  association [0..1] to /PLCE/R_PDTourTemplate as _TourTemplate     on $projection.TourTemplate = _TourTemplate.TourTemplate
  association [0..*] to /PLCE/P_PDTourStatusT  as _PDTourStatusText on $projection.TourStatus = _PDTourStatusText.Value

{
      @Semantics.uuid: true
      @UI.hidden: true
  key tour_uuid        as TourUuid,
      @EndUserText.label: 'Tour ID'
      tour_id          as TourId,
      tour_template    as TourTemplate,
      tour_status      as TourStatus,
      @EndUserText.label: 'Scheduled Start'
      @Semantics.dateTime: true
      date_time_start  as ScheduledDateTimeStart,
      @EndUserText.label: 'Tour Start Date'
      date_start       as StartDate,
      @EndUserText.label: 'Tour End Date'
      date_end         as EndDate,
      @EndUserText.label: 'Main Resource'
      main_resource_id as MainResourceId,

      _TourLookup,
      _TourTemplate,
      _PDTourStatusText,
      _ServiceAssignments,
      _Attachments
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'R View on /PLCE/TPDTASSRVC'
@Metadata.ignorePropagatedAnnotations: true
define view entity ZR_PDTOURSERVICEASGMT
  as select from /plce/tpdtassrvc
  association        to parent ZR_PDTOUR             as _Tour                      on  $projection.TourUuid = _Tour.TourUuid
  association [0..1] to /PLCE/R_PDService            as _Service                   on  _Service.ServiceUUID = $projection.ServiceUuid
  association [0..1] to /PLCE/R_PDTourTaskAssignment as _LeadServiceTaskAssignment on  _LeadServiceTaskAssignment.TourUUID        = $projection.TourUuid
                                                                                   and _LeadServiceTaskAssignment.TaskScope       = 'S'
                                                                                   and _LeadServiceTaskAssignment.ServiceUUID     = $projection.ServiceUuid
                                                                                   and _LeadServiceTaskAssignment.ServiceTaskUUID = $projection.LeadServiceTaskUuid
  association [1..1] to /PLCE/R_PDTourServiceAsgmt_L as _Lookup                    on  _Lookup.TourUUID    = $projection.TourUuid
                                                                                   and _Lookup.ServiceUUID = $projection.ServiceUuid

{
  key tour_uuid                               as TourUuid,
  key service_uuid                            as ServiceUuid,
      lead_service_task_uuid                  as LeadServiceTaskUuid,
      _LeadServiceTaskAssignment.TourSequence as TourSequence,
      @EndUserText.label: 'Removed'
      removed                                 as Removed,

      _Tour,
      _Service,
      _Lookup,
      _LeadServiceTaskAssignment
} where removed <> 'X'




managed implementation in class zbp_r_pdtour unique;
strict ( 2 );

define behavior for ZR_PDTOUR alias Tour
persistent table /plce/tpdtour
lock master
authorization master ( global )
{
  field ( readonly ) TourUuid;

  association _Attachments;
  association _ServiceAssignments;

  action ( features : instance ) RegenerateDocs;
  static action generatedocuments parameter ZAE_ATTACHTMENT_PARAM result [1] $self;
  side effects
  {
    action RegenerateDocs affects entity _Attachments;
  }

  mapping for /plce/tpdtour corresponding
    {
      TourUuid               = tour_uuid;
      TourId                 = tour_id;
      TourTemplate           = tour_template;
      TourStatus             = tour_status;
      ScheduledDateTimeStart = date_time_start;
      MainResourceId         = main_resource_id;
    }
}

define behavior for ZR_PDATTACHMENT alias Attachment
persistent table /plce/tpdtrattm
lock dependent by _Tour
authorization dependent by _Tour
{

  field ( readonly ) Uuid, AttachmentUuid, TourUUID, CreatedBy, CreatedAt;

  action deleteAttachment result [0..*] $self;
  delete;
  association _Tour;

  mapping for /plce/tpdtrattm corresponding
    {
      TourUUID       = tour_uuid;
      AttachmentUuid = attachment_uuid;
      CreatedAt      = created_at;
      CreatedBy      = created_by;
    }
}

define behavior for ZR_PDTOURSERVICEASGMT alias ServiceAssignment
persistent table /plce/tpdtassrvc
lock dependent by _Tour
authorization dependent by _Tour
{
  field ( readonly ) ServiceUuid, TourUuid;
  association _Tour;

  mapping for /plce/tpdtassrvc corresponding
    {
      TourUuid            = tour_uuid;
      ServiceUuid         = service_uuid;
      LeadServiceTaskUuid = lead_service_task_uuid;
    }
}


projection;
strict ( 2 );
use side effects;

define behavior for ZC_PDTOUR alias Tour
{
  use action RegenerateDocs;
  use action generatedocuments;

  use association _Attachments;
  use association _ServiceAssignments;
}

define behavior for ZC_PDATTACHMENT alias Attachment
{
  use action deleteAttachment;
  use association _Tour;
}

define behavior for ZC_PDTOURSERVICEASGMT alias ServiceAssignment
{

  use association _Tour;
}

