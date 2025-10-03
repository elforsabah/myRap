@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Service',
    typeNamePlural: 'Services',
    title: {
      type: #STANDARD, value: 'ServiceId'
    },
    description: {
      type: #STANDARD, value: 'ServiceTypeDescription'
    }
  },
  presentationVariant: [{
    sortOrder: [{
      by: 'ServiceId', direction:  #DESC
    }],
   visualizations: [{ type: #AS_LINEITEM }]
  }]
}

annotate entity /PLCE/C_PDMNLServiceWR with
{

  @UI.facet: [{
      id: 'F0_CoreDefault',
      type: #FIELDGROUP_REFERENCE,
      targetElement: '_ServiceCore',
      targetQualifier: 'DefaultInformation'
    },
    { 
      id: 'F1_CoreDefault',
      type: #FIELDGROUP_REFERENCE,
      targetElement: '_ServiceCore',
      targetQualifier: 'GeneralInformation'    
    },
    { 
      id: 'F2_CoreDefault',
      type: #FIELDGROUP_REFERENCE,
      targetElement: '_ServiceCore',
      targetQualifier: 'Location'    
    },
    { 
      id: 'F3_CoreDefault',
      type: #FIELDGROUP_REFERENCE,
      targetElement: '_ServiceCore',
      targetQualifier: 'Scheduling'    
    },
    { 
      id: 'F4_CoreDefault',
      type: #FIELDGROUP_REFERENCE,
      targetElement: '_ServiceCore',
      targetQualifier: 'Admin'    
    },
    {
      id: 'F10_WRDefault',
      type: #FIELDGROUP_REFERENCE,
      targetQualifier: 'WR_Default'
    }]



  @UI: {  lineItem: [ { position: 1 },
                      { hidden: true },
                      { type:#FOR_ACTION, dataAction:'AssignTour', label:'Assign Tour', invocationGrouping: #CHANGE_SET },
                      { type:#FOR_ACTION, dataAction:'CreateTour', label:'Create Tour', invocationGrouping: #CHANGE_SET }                                                                                                         
//                      { type:#FOR_ACTION, dataAction:'PlanAutomatically', label: 'Auto-Plan', invocationGrouping: #CHANGE_SET}
                    ] }
  ServiceUUID;

  @UI.lineItem: [{ position: 10, importance: #HIGH }]  
  ReferenceId;

  @UI.fieldGroup: [{position: 20, qualifier: 'DefaultInformation' }]
  ServiceId;
  
  @UI.fieldGroup: [{position: 50, qualifier: 'DefaultInformation' }]
  AdditionalText;

  @UI.lineItem: [{ position: 30, importance: #HIGH, cssDefault.width: '5rem'  }]
  WorkStatusIcon;

  @UI.lineItem: [{ position: 40, importance: #HIGH, cssDefault.width: '5rem'  }]
  ServiceStatusIcon;

  @UI.lineItem: [{ position: 50, importance: #HIGH }]
  CustomerInfo;

  @UI: { lineItem: [{ position: 60, importance: #HIGH }] }
  FullAddress;

  @UI.lineItem: [{ position: 70, importance: #HIGH }]
  @UI.textArrangement: #TEXT_ONLY
  ServiceType;

  @UI: { lineItem: [{ position: 80, type: #AS_FIELDGROUP, valueQualifier: 'lfg_containertypeat', label: 'ContType At Loc #'}] }
  @UI.fieldGroup: [ {qualifier: 'lfg_containertypeat', position: 10 }]  
  ContainerTypeAtLocation;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [ {position: 30, qualifier: 'WR_Default', type: #AS_CONNECTED_FIELDS, valueQualifier: 'cf_containertypeat' }]
  @UI.connectedFields: [{ qualifier: 'cf_containertypeat', groupLabel: 'Container Type At Location', name: 'containertype',  template: '{containertype_count} {containertype}' }]
  ContainerTypeAtLocationWiText;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{ qualifier: 'lfg_containertypeat', position: 20 }]
  @UI.connectedFields: [{ qualifier: 'cf_containertypeat', name: 'containertype_count' }]
  ContainerAtLocationCount;


  @UI: { lineItem: [{ position: 90, type: #AS_FIELDGROUP, valueQualifier: 'lfg_containertypenew', label: 'Container Type New #' }] }
  @UI.fieldGroup: [ {qualifier: 'lfg_containertypenew', position: 10 }]  
  ContainerTypeNew;
  @UI.lineItem: [{ hidden: true }]  
  @UI.fieldGroup: [{position: 40, qualifier: 'WR_Default', type: #AS_CONNECTED_FIELDS, valueQualifier: 'cf_containertypenew' }]
  @UI.connectedFields: [{ qualifier: 'cf_containertypenew', groupLabel: 'Container Type New', name: 'containertype',  template: '{containertype_count} {containertype}' }]
  ContainerTypeNewWithText;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{ qualifier: 'lfg_containertypenew', position: 20 }]
  @UI.connectedFields: [{ qualifier: 'cf_containertypenew', name: 'containertype_count' }]
  ContainerNewCount;

  @UI: { lineItem: [{ position: 100, type: #AS_FIELDGROUP, valueQualifier: 'lfg_material', label: 'Material #' }] }
  @UI.fieldGroup: [{qualifier: 'lfg_material', position: 10 }]    
  Material;
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{position: 50, qualifier: 'WR_Default', type: #AS_CONNECTED_FIELDS, valueQualifier: 'cf_material' }]
  @UI.connectedFields: [{ qualifier: 'cf_material', groupLabel: 'Material', name: 'material',  template: '{material_weight} {material}' }]
  MaterialWithText;
  
  @UI.fieldGroup: [{position: 51, qualifier: 'WR_Default'  }]  
  MaterialText;
  
  
  @UI.lineItem: [{ hidden: true }]
  @UI.fieldGroup: [{ qualifier: 'lfg_material', position: 20 }]
  @UI.connectedFields: [{ qualifier: 'cf_material', name: 'material_weight' }]
  MaterialWeight;

  @UI.lineItem: [{ position: 110 }]
  @UI.selectionField: [{ position: 40 }]
  MaterialGroup;

  @UI.lineItem: [{ position: 120 }]
  @UI.selectionField: [{ position: 10 }]
  RequestedDate;

  @UI.lineItem: [{ position: 130 }]
  TotalDuration;

  @UI.lineItem: [{ position: 140, criticality: 'PlanningStatusCriticality' }]
  @UI.selectionField: [{ position: 20 }]
  PlanningStatus;
  
  @UI.lineItem: [{ position: 150 }]
  TourId;
  
  @UI.lineItem: [{ position: 160 }]
  ServiceFrequencyText;
  
  @UI.selectionField: [{ element: '_WorkAreaServices.WorkArea', position : 1 }]
  _WorkAreaServices;
  @UI.selectionField: [{ element: '_TourAssignments.TourId', position : 30 }]
  _TourAssignments;
    
  @UI.lineItem: [{ hidden: true }]
  MapTitle;

  @UI.lineItem: [{ hidden: true }]
  MapPopup;
 
  @UI.lineItem: [{ hidden: true }]
  MapColor;
          
  @UI.lineItem: [{ hidden: true }]
  MapHighlight;
           
  @UI.lineItem: [{ hidden: true }]
  MapSymbol;
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
@AccessControl.authorizationCheck: #NOT_REQUIRED
@ClientHandling.type: #CLIENT_DEPENDENT
@ClientHandling.algorithm: #SESSION_VARIABLE
@EndUserText.label: 'Work Area Service - Table Function'
define table function /PLCE/P_PDWorkAreaServiceFunc
  with parameters
    @Environment.systemField: #CLIENT
    p_client : abap.clnt
returns
{
  mandt        : abap.clnt;
  work_area    : /plce/pdwork_area;
  service_uuid : /plce/pdservice_uuid;
}
implemented by method
  /PLCE/CL_PD_WORKAREA_FUNC=>GET_WORKAREAS_SERVICES;

    method get_workareas_services
    by database function for hdb
        language sqlscript
        options read-only
        using /plce/r_pdservice /plce/r_pdfunctionallocation /plce/r_pdworkareaprofile /plce/r_pdworkareapostalcode.
*   "*" not work for like, only "%" is allowed
    return select distinct SRVC.MANDT, WAPC.WORKAREA AS WORK_AREA, SRVC.SERVICEUUID AS SERVICE_UUID
    FROM "/PLCE/R_PDSERVICE" as SRVC
    INNER JOIN "/PLCE/R_PDWORKAREAPROFILE" as WAPR on WAPR.profile like SRVC.profile
*    INNER JOIN "/PLCE/R_PDWORKAREAPROFILE" as WAPR on SRVC.profile like ( CASE when WAPR.profile is NULL then '%' else WAPR.profile end )
    INNER JOIN "/PLCE/R_PDFUNCTIONALLOCATION" as FL on FL.functionallocation = SRVC.functionallocation
    INNER JOIN "/PLCE/R_PDWORKAREAPOSTALCODE" as WAPC on WAPC.workarea = WAPR.workarea and FL.postalcode like WAPC.postalcodesql
*    INNER JOIN "/PLCE/R_PDWORKAREAPOSTALCODE" as WAPC on WAPC.workarea = WAPR.workarea and FL.postalcode like ( CASE when WAPC.postalcodesql is NULL then '%' else WAPC.postalcodesql end )
*    INNER JOIN "/PLCE/R_PDWORKAREA" as WA on WA.workarea = WAPR.workarea
    WHERE SRVC.MANDT = :P_CLIENT;
*    SESSION_CONTEXT('CDS_CLIENT');
  endmethod.


