@EndUserText.label: 'Parameters for Attachment'
define abstract entity ZAE_ATTACHTMENT_PARAM
{
    AttachmentItemsjson : abap.string(0);
    ServiceWRItemsjson  : abap.string(0);
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Service Attachments'
@Metadata.allowExtensions: true
define view entity ZC_PDATTACHMENT
  as projection on ZR_PDATTACHMENT
{
      @UI.hidden: true
  key Uuid,
      @UI.hidden: true
  key AttachmentUuid,
      @UI.hidden: true
  key TourUUID,
      _Tour.TourId,
      TourSequence,
      ReferenceId,
      Filename,
      @UI.hidden: true
      Mimetype,
      Attachment,
      CreatedBy,
      CreatedAt,
      /* Associations */
      _Tour : redirected to parent ZC_PDTOUR
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Consumption View (Tour WR)'
@Search.searchable: true
@Metadata.allowExtensions: true
define root view entity ZC_PDTOUR
  provider contract transactional_query
  as projection on ZR_PDTOUR

{
  key TourUuid,
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourWA_VH', element: 'TourId' } }]
      @Search: { defaultSearchElement: true, ranking: #HIGH }
      TourId,

      @EndUserText.label: 'Tour Template'
      @ObjectModel.text.element: ['TourTemplateName']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourTemplateWA_VH', element: 'TourTemplate' } }]
      @Search: { defaultSearchElement: true }
      TourTemplate,
      @EndUserText.label: 'Template Description'
      @Consumption.filter.hidden: true
      @Semantics.text: true
      _TourTemplate._Text.TourTemplateName         as TourTemplateName : localized,
      @UI.hidden: true
      _TourTemplate.ColorTour                      as ColorTour,

      @EndUserText.label: 'Tour Status'
      @ObjectModel.text.element: ['TourStatusText']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourStatus_VH', element: 'TourStatus' } }]
      @Consumption.filter: { multipleSelections: true, selectionType: #SINGLE }
      TourStatus,
      @Semantics.text: true
      @UI.hidden: true
      _PDTourStatusText.Description                as TourStatusText   : localized,
      @UI.hidden: true
      _TourLookup.TourStatusColorValue             as TourStatusColorValue,
      @Consumption.filter.hidden: true
      _PDTourStatusText.IconURL                    as TourStatusIcon   : localized,

      @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
      StartDate                                    as TourStartDate,
      @Consumption.filter.selectionType: #INTERVAL
      EndDate                                      as TourEndDate,
      @Consumption.filter.selectionType: #INTERVAL
      ScheduledDateTimeStart,

      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDResource_VH', element: 'ResourceId'} }]
      MainResourceId,

      @EndUserText.label: 'Work Status'
      @UI.textArrangement: #TEXT_ONLY
      @ObjectModel.text.element: ['WorkStatusText']
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value'} }]
      _TourLookup.WorkStatus,
      @UI.hidden: true
      _TourLookup._WorkStatusText.Description      as WorkStatusText   : localized,
      @Consumption.filter.hidden: true
      _TourLookup._WorkStatusText.IconURL          as WorkStatusIcon   : localized,
      _TourLookup._TourCriticality.TourCriticality as TourCriticality,

      /* Associations */
      _Attachments        : redirected to composition child ZC_PDATTACHMENT,
      @Consumption.filter.hidden:false
      _ServiceAssignments : redirected to composition child ZC_PDTOURSERVICEASGMT
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Service Assignments'
@Search.searchable: true
@Metadata.allowExtensions: true
define view entity ZC_PDTOURSERVICEASGMT
  as projection on ZR_PDTOURSERVICEASGMT
{
      @UI.hidden: true
  key TourUuid,
      @UI.hidden: true
  key ServiceUuid,
      _Tour.TourId                                                                           as TourId,
      _Service.ServiceId                                                                     as ServiceId,

      @ObjectModel.text.element: ['ServiceTypeText']
      _Service.ServiceType                                                                   as ServiceType,
      _Service._ServiceType._ServiceTypeText.ServiceTypeDescription                          as ServiceTypeText             : localized,

      @EndUserText.label: 'Service Status Description'
      @ObjectModel.text.element: ['ServiceStatusText']
      @UI.textArrangement: #TEXT_ONLY
      _Service.ServiceStatus,
      @UI.hidden: true
      _Service._ServiceStatusText[1:Language = $session.system_language].Description         as ServiceStatusText,
      _Service._ServiceStatusText[1:Language = $session.system_language].IconURL             as ServiceStatusIcon,

      @UI.hidden: true
      Removed,
      TourSequence,

      @Search.defaultSearchElement: true
      _Service.ReferenceId                                                                   as ReferenceId,

      @Search.defaultSearchElement: true
      _Service.CustomerInfo                                                                  as CustomerInfo,
      _Service.AdditionalText                                                                as AdditionalText,

      _Service.FunctionalLocation                                                            as FunctionalLocation,
      _Service._FunctionalLocation.FullAddress                                               as FullAddress,

      //Extension WR
      @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
      _Service._ExtWaste.Material                                                            as Material,
      @ObjectModel.text.element: ['MaterialText']
      @UI.textArrangement: #TEXT_LAST
      _Service._ExtWaste.Material                                                            as MaterialWithText,

      @EndUserText.label: 'Material Description'
      _Service._ExtWaste._Material._Text.MaterialName                                        as MaterialText                : localized,
      @Semantics.quantity.unitOfMeasure:'MaterialWeightUnit'
      _Service._ExtWaste.MaterialWeight                                                      as MaterialWeight,
      @UI.hidden: true
      cast( _Service._ExtWaste.MaterialWeightUnit as meins preserving type )                 as MaterialWeightUnit, //show localized key/allow fiori app value check
      @ObjectModel.text.element: [ 'MaterialGroupText' ]
      @EndUserText.label: 'Material Group'
      @UI.textArrangement: #TEXT_LAST
      _Service._ExtWaste._Material.MaterialProductGroup                                      as MaterialGroup,
      @EndUserText.label: 'Material Group Description'
      _Service._ExtWaste._Material._ProductGroup._ProductGroupText.ProductGroupName          as MaterialGroupText           : localized,

      @ObjectModel.text.element: ['PlantLocationText']
      _Service._ExtWaste.PlantLocation                                                       as PlantLocation,
      @UI.hidden: true
      _Service._ExtWaste._PlantLocation._FuctionalLocationText.FunctionalLocationDescription as PlantLocationText           : localized,

      _Service._ExtWaste.ContainerSourceLocation                                             as ContainerSourceLocation,
      @EndUserText.label: 'Address - Container Source'
      _Service._ExtWaste._ContainerSourceLocation.FullAddress                                as ContainerSourceAddress,
      _Service._ExtWaste.ContainerFinalLocation                                              as ContainerFinalLocation,
      @EndUserText.label: 'Address - Container Final'
      _Service._ExtWaste._ContainerFinalLocation.FullAddress                                 as ContainerFinalAddress,

      @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
      _Service._ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocation,
      @ObjectModel.text.element: ['ContainerTypeAtLocationText']
      @UI.textArrangement: #TEXT_LAST
      @Consumption.filter.hidden: true
      _Service._ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocationWiText,

      @UI.hidden: true
      _Service._ExtWaste._ContainerTypeAtLocation._Text.TransportPackagingTypeName           as ContainerTypeAtLocationText : localized,

      _Service._ExtWaste.ContainerAtLocationCount                                            as ContainerAtLocationCount,
      _Service._ExtWaste.ContainerAtLocationTidnr                                            as ContainerAtLocationTidnr,

      @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
      _Service._ExtWaste.ContainerTypeNew                                                    as ContainerTypeNew,
      @ObjectModel.text.element: ['ContainerTypeNewText']
      @UI.textArrangement:  #TEXT_LAST

      @Consumption.filter.hidden: true
      _Service._ExtWaste.ContainerTypeNew                                                    as ContainerTypeNewWithText,
      @UI.hidden: true
      _Service._ExtWaste._ContainerTypeNew._Text.TransportPackagingTypeName                  as ContainerTypeNewText        : localized,
      _Service._ExtWaste.ContainerNewCount                                                   as ContainerNewCount,
      _Service._ExtWaste.ContainerNewTidnr                                                   as ContainerNewTidnr,
      @UI.hidden: true
      _Service._ExtWaste.ServiceFrequency                                                    as ServiceFrequency,
      @EndUserText.label: 'Service Frequency'
      _Service._ExtWaste._ServiceFrequencyText.ServiceFrequencyText                          as ServiceFrequencyText        : localized,
      //Extension WR


      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      _Service._Statistic.TotalDuration,
      @UI.hidden: true
      cast( _Service._Statistic.TotalDurationUnit as meins preserving type )                 as TotalDurationUnit, //show localized key/allow fiori app value check

      @EndUserText.label: 'Work Status Description'
      @ObjectModel.text.element: ['WorkStatusText']
      @UI.textArrangement: #TEXT_ONLY
      _Lookup.WorkStatus,
      @UI.hidden: true
      _Lookup._WorkStatusText.Description                                                    as WorkStatusText              : localized,
      _Lookup._WorkStatusText.IconURL                                                        as WorkStatusIcon              : localized,

      _Service.PlanningStatus,

      /* Associations */
      _Service : redirected to /PLCE/C_PDMNLServiceWR,
      _Tour    : redirected to parent ZC_PDTOUR
}


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
}


@EndUserText.label: 'Service WR'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@Search.searchable: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLServiceWR
  provider contract transactional_query
  as projection on /PLCE/R_PDService
  association [1] to /PLCE/C_PDMNLService as _ServiceCore on _ServiceCore.ServiceUUID = $projection.ServiceUUID
  //association [1] to /PLCE/C_PDServiceC as _ServiceCore on _ServiceCore.ServiceUUID = $projection.ServiceUUID
{
          @UI.hidden: true
  key     ServiceUUID,

          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.7 }
          //          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDService_VH', element: 'ServiceId' } }]
          ServiceId,
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDProfileWA_VH', element: 'Profile' } }]
          Profile,

          @ObjectModel.text.element: ['ServiceTypeDescription']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDServiceTypeWA_VH', element: 'ServiceType'  } }]
          ServiceType,

          @UI.hidden: true
          _ServiceType._ServiceTypeText.ServiceTypeDescription                          as ServiceTypeDescription      : localized,


          @EndUserText.label: 'Service Status'
          @ObjectModel.text.element: ['ServiceStatusText']
          @UI.textArrangement: #TEXT_ONLY
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDServiceStatus_VH', element: 'ServiceStatus' } }]
          ServiceStatus,
          @UI.hidden: true
          _ServiceStatusText.Description                                                as ServiceStatusText           : localized,
          @Consumption.filter.hidden: true
          _ServiceStatusText.IconURL                                                    as ServiceStatusIcon           : localized,

          @Search:{ defaultSearchElement: true  }
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDOrderWAWR_VH', element: 'ReferenceId' } }]
          ReferenceId,
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDServicePriority_VH', element: 'ServicePriority' } }]
          ServicePriority,

          @UI.hidden: true
          _PlanningStatusText.Description                                               as PlanningStatusText          : localized,

          @ObjectModel.text.element: ['PlanningStatusText']
          @UI.textArrangement: #TEXT_ONLY
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDPlanningStatus_VH', element : 'PlanningStatus'} }]
          @Consumption.filter: { defaultValue: '', multipleSelections: true, selectionType: #SINGLE }
          PlanningStatus,

          @UI.hidden: true
          _ServiceLookup.PlanningStatusCriticality                                      as PlanningStatusCriticality,

          @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
          RequestedDate,
          @Consumption.filter.selectionType: #INTERVAL
          EarliestDate,
          @Consumption.filter.selectionType: #INTERVAL
          LatestDate,

          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.7 }
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDCustomerInfoWA_VH', element: 'CustomerInfo'} }]
          CustomerInfo                                                                  as CustomerInfo,

          AdditionalText,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FunctionalLocation' } }]
          @EndUserText.label: 'Customer Location'
          FunctionalLocation,
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FullAddress' } }]
          _FunctionalLocation.FullAddress                                               as FullAddress,

          @UI.hidden: true
          @Semantics.unitOfMeasure: true
          cast( _Statistic.TotalDurationUnit as meins preserving type )                 as TotalDurationUnit, //show localized key/allow fiori app value check
          @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
          @Consumption.filter.hidden: true
          _Statistic.TotalDuration,

          @EndUserText.label: 'Work Status'
          @ObjectModel.text.element: ['WorkStatusText']
          @UI.textArrangement: #TEXT_ONLY
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value' } }]
          _ServiceLookup.WorkStatus,
          @UI.hidden: true
          _ServiceLookup._WorkStatusText.Description                                    as WorkStatusText              : localized,
          @Consumption.filter.hidden: true
          _ServiceLookup._WorkStatusText.IconURL                                        as WorkStatusIcon              : localized,

          @ObjectModel.text.element: ['ServiceWindowText']
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDServiceWindow_VH', element : 'ServiceWindow'} }]
          @UI.textArrangement: #TEXT_ONLY
          ServiceWindow,
          @UI.hidden: true
          _ServiceWindowText[1:Language = $session.system_language].Description         as ServiceWindowText,
          @Consumption.filter.selectionType: #SINGLE
          ServiceWindowStartTime,
          @Consumption.filter.selectionType: #SINGLE
          ServiceWindowEndTime,

          @Consumption.filter.hidden: true
          _TourAssignments[1:ServiceUUID = $projection.serviceuuid].TourId              as TourId,

          //Extension Waste
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MDMATERIAL_VH', element: 'ResourceKey'} }]
          @Search:{ defaultSearchElement: true } // fuzzinessThreshold: 0.8, ranking: #LOW
          _ExtWaste.Material                                                            as Material,
          @ObjectModel.text.element: ['MaterialText']
          @UI.textArrangement: #TEXT_LAST
          @Consumption.filter.hidden: true
          _ExtWaste.Material                                                            as MaterialWithText,

          @EndUserText.label: 'Material Description'
          @Consumption.filter.hidden: true
          _ExtWaste._Material._Text.MaterialName                                        as MaterialText                : localized,
          @Semantics.quantity.unitOfMeasure:'MaterialWeightUnit'
          @Consumption.filter.hidden: true
          _ExtWaste.MaterialWeight                                                      as MaterialWeight,
          @UI.hidden: true
          cast( _ExtWaste.MaterialWeightUnit as meins preserving type )                 as MaterialWeightUnit, //show localized key/allow fiori app value check
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MaterialGroup_VH', element: 'MaterialGroup'} }]
          @ObjectModel.text.element: [ 'MaterialGroupText' ]
          @EndUserText.label: 'Material Group'
          @UI.textArrangement: #TEXT_LAST
          _ExtWaste._Material.MaterialProductGroup                                      as MaterialGroup,
          @EndUserText.label: 'Material Group Description'
          @UI.hidden: true
          _ExtWaste._Material._ProductGroup._ProductGroupText.ProductGroupName          as MaterialGroupText           : localized,

          @ObjectModel.text.element: ['PlantLocationText']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDNoCustomerLocWA_VH', element: 'FunctionalLocation' } }]
          @EndUserText.label: 'Plant Location'
          _ExtWaste.PlantLocation                                                       as PlantLocation,
          @UI.hidden: true
          _ExtWaste._PlantLocation._FuctionalLocationText.FunctionalLocationDescription as PlantLocationText           : localized,

          @ObjectModel.text.element: ['ContainerSourceAddress']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocWA_VH', element: 'FunctionalLocation'} }]
          @EndUserText.label: 'Address - Container Source'
          _ExtWaste.ContainerSourceLocation                                             as ContainerSourceLocation,
          @UI.hidden: true
          _ExtWaste._ContainerSourceLocation.FullAddress                                as ContainerSourceAddress,
          @ObjectModel.text.element: ['ContainerFinalAddress']
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocWA_VH', element: 'FunctionalLocation'} }]
          @EndUserText.label: 'Address - Container Final'
          _ExtWaste.ContainerFinalLocation                                              as ContainerFinalLocation,
          @UI.hidden: true
          _ExtWaste._ContainerFinalLocation.FullAddress                                 as ContainerFinalAddress,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MDTransptPackaggT_VH', element: 'ResourceKey' } }]
          //          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
          _ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocation,
          @ObjectModel.text.element: ['ContainerTypeAtLocationText']
          @UI.textArrangement: #TEXT_LAST
          @Consumption.filter.hidden: true
          _ExtWaste.ContainerTypeAtLocation                                             as ContainerTypeAtLocationWiText,

          @UI.hidden: true
          _ExtWaste._ContainerTypeAtLocation._Text.TransportPackagingTypeName           as ContainerTypeAtLocationText : localized,

          _ExtWaste.ContainerAtLocationCount                                            as ContainerAtLocationCount,
          _ExtWaste.ContainerAtLocationTidnr                                            as ContainerAtLocationTidnr,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_MDTransptPackaggT_VH', element: 'ResourceKey' } }]
          //          @Search:{ defaultSearchElement: true, fuzzinessThreshold: 0.8, ranking: #LOW }
          @ObjectModel.text.element: ['ContainerTypeNewText']
          _ExtWaste.ContainerTypeNew                                                    as ContainerTypeNew,
          @ObjectModel.text.element: ['ContainerTypeNewText']
          @UI.textArrangement:  #TEXT_LAST

          @Consumption.filter.hidden: true
          _ExtWaste.ContainerTypeNew                                                    as ContainerTypeNewWithText,
          @UI.hidden: true
          _ExtWaste._ContainerTypeNew._Text.TransportPackagingTypeName                  as ContainerTypeNewText        : localized,
          _ExtWaste.ContainerNewCount                                                   as ContainerNewCount,
          _ExtWaste.ContainerNewTidnr                                                   as ContainerNewTidnr,
          @UI.hidden: true
          _ExtWaste.ServiceFrequency                                                    as ServiceFrequency,
          @EndUserText.label: 'Service Frequency'
          _ExtWaste._ServiceFrequencyText.ServiceFrequencyText                          as ServiceFrequencyText        : localized,

         //map
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapTitle  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapPopup  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapColor  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapHighlight  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
          virtual MapSymbol  : abap.string,

          /* Associations */
          _ServiceTask        : redirected to composition child /PLCE/C_PDMNLServiceTaskWR,
          //_Attachments        : redirected to composition child /PLCE/C_PDMNLSERVICEATTACHMENT,
          _FunctionalLocation : redirected to /PLCE/C_PDMNLFuncLoc,
          _WorkAreaServices,
          _TourAssignments, //    : redirected to /PLCE/C_PDMNLTourServiceAsgWR,
          //_PlanningStatusText,
          _ServiceCore

}
@EndUserText.label: 'TourTemplate Value Help'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@Search.searchable: true
@ObjectModel.usageType:{
  serviceQuality: #A,
  sizeCategory: #S,
  dataClass: #MASTER
}
define view entity /PLCE/C_PDTourTemplateWA_VH
  as select distinct from /PLCE/R_PDTourTemplate     as TourTemplate
    inner join            /PLCE/P_PDWorkAreaTourTmpl as WorkAreaTourTmpl on WorkAreaTourTmpl.TourTemplate = TourTemplate.TourTemplate
{
      @UI.hidden: true
  key WorkAreaTourTmpl.WorkArea,
      @ObjectModel.text.element: ['TourTemplateName']
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #HIGH
  key TourTemplate.TourTemplate,
      @EndUserText.label: 'Description'
      @Semantics.text: true
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #MEDIUM
      TourTemplate._Text[1:Language = $session.system_language].TourTemplateName
}
@EndUserText.label: 'Tour Value Help'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.allowExtensions: true
@Search.searchable: true
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define view entity /PLCE/C_PDTourWA_VH
  as select distinct from /PLCE/R_PDTour         as Tour
    inner join            /PLCE/P_PDWorkAreaTour as WorkArea     on Tour.TourUUID = WorkArea.TourUUID
{
      @Semantics.uuid: true
  key Tour.TourUUID,
      @UI.hidden: true
  key WorkArea.WorkArea,
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.9
      @Search.ranking: #HIGH
      Tour.TourId,
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #MEDIUM
      Tour.TourTemplate,
      @ObjectModel.text.element: ['TourStatusText']
      Tour.TourStatus,
      @EndUserText.label: 'Tour Status'
      @Semantics.text: true
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
      @Search.ranking: #LOW
      Tour._PDTourStatusText[1:Language = $session.system_language].Description as TourStatusText,
      Tour.StartDate,
      Tour.EndDate,
      @UI.hidden: true
      Tour._TourLookup.WorkStatus,

      /* Associations */
      //      _ServiceTask,
      //      _TourTask,
      //      _TourTemplate,
      Tour._TourLookup,
      Tour._PDTourStatusText,
      Tour._WorkAreaTours,
      Tour._Profiles
}
where
  Tour.CreatedForSimulation is initial
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Status Value Help'
@Metadata.allowExtensions: true
@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #S,
  dataClass: #MASTER
}
define view entity /PLCE/C_PDTourStatus_VH 
    as select from /PLCE/P_PDTourStatusT
{
        @ObjectModel.text.element: ['StatusDescription']
        @UI.lineItem: [{ position: 10 }]
        @UI.textArrangement: #TEXT_ONLY
    key Value as TourStatus,
        @UI.hidden:true
    key Language,
        @Semantics.text: true
        @UI.hidden: true
        Description as StatusDescription

}
 where Language = $session.system_language
@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Main Resource Value Help'
@Metadata.ignorePropagatedAnnotations: true
@Search.searchable: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #M,
  dataClass: #MIXED
}
define view entity /PLCE/C_PDResource_VH 
    as select from /PLCE/R_MDResource
{
    @Search.defaultSearchElement: true
    @UI.lineItem: [{position: 10 }]
    key ResourceId,
    
    @UI.lineItem: [{position: 20 }]
    SemanticContext,
    
//    @UI.lineItem: [{position: 30 }]
//    Equipment,
//    
//    @UI.lineItem: [{position: 40 }]
//    Partner,
    
//    @UI.lineItem: [{position: 30 }]
//    CommunicationUser,
    
    @UI.lineItem: [{position: 40 }]
    Description
}
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Work Status Value Help'
@Metadata.allowExtensions: true
@ObjectModel.resultSet.sizeCategory: #XS
@ObjectModel.usageType:{
    serviceQuality: #C,
    sizeCategory: #S,
    dataClass: #MASTER
}
define view entity /PLCE/C_PDWorkStatus_VH 
  as select from /PLCE/P_PDWorkStatusT
{
      @ObjectModel.text.element: ['Description']
      @UI.lineItem: [{ position: 10 }]
      @UI.textArrangement: #TEXT_ONLY
  key Value,
      @Semantics.language: true
      @UI.hidden: true
  key Language,
      @UI.hidden: true
      @Semantics.text: true
      Description,
      @Semantics.imageUrl : true
      @ObjectModel.text.element: ['Description']
      @UI.textArrangement: #TEXT_ONLY 
      @UI.hidden: true
      IconURL   
}
where Language = $session.system_language
