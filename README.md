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
  association [1]    to /PLCE/C_PDMNLService          as _ServiceCore       on _ServiceCore.ServiceUUID = $projection.ServiceUUID
  //association [1] to /PLCE/C_PDServiceC as _ServiceCore on _ServiceCore.ServiceUUID = $projection.ServiceUUID
  association [0..*] to /PLCE/C_PDCFServiceConfNoteWR as _ServiceConfNoteWR on _ServiceConfNoteWR.ServiceUUID = $projection.ServiceUUID
  association [0..*] to /PLCE/C_PDCFServiceDocResWR   as _ServiceDocumentWR on _ServiceDocumentWR.ServiceUUID = $projection.ServiceUUID
{
//          @UI.hidden: true
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
          @UI.multiLineText: true
          AdditionalText,

          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FunctionalLocation' } }]
          @EndUserText.label: 'Customer Location'
          FunctionalLocation,
          @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FullAddress' } }]
          _FunctionalLocation.FullAddress                                               as FullAddress,

          @Consumption.valueHelpDefinition: [{ entity: { name : '/PLCE/C_MDPLANT_VH', element: 'Plant' } }]
          PlanningPlant,

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
          @ObjectModel.text.element: ['ContainerTypeAtLocationText']
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
          @UI.textArrangement: #TEXT_LAST
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

          @EndUserText.label: 'Replanned'
          _ServiceDeviation[1:ReplannedFlag is not initial].ReplannedFlag               as ReplannedFlag,
          //map
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
  virtual MapTitle      : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
  virtual MapPopup      : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
  virtual MapColor      : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
  virtual MapHighlight  : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_SERVICE_CALC'
  virtual MapSymbol     : abap.string,

          /* Associations */
          _ServiceTask        : redirected to composition child /PLCE/C_PDMNLServiceTaskWR,
          //_Attachments        : redirected to composition child /PLCE/C_PDMNLSERVICEATTACHMENT,
          _FunctionalLocation : redirected to /PLCE/C_PDMNLFuncLoc,
          _WorkAreaServices,
          _TourAssignments, //    : redirected to /PLCE/C_PDMNLTourServiceAsgWR,
          //_PlanningStatusText,
          _ServiceCore,
          _ServiceConfNoteWR,
          _ServiceDocumentWR
}
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


@Metadata.layer: #CUSTOMER
@UI: {
lineItem: [{ criticality: 'service_criticality' }]
}
annotate entity /PLCE/C_PDMNLServiceWR with
{

  @UI.hidden: true
  service_criticality;
  @UI.lineItem: [ { position: 70, importance: #HIGH },
  {
  type:#FOR_ACTION,
  dataAction:'changeServiceType',
  label: 'Change ServiceType',
  iconUrl: 'sap-icon://wrench',
  inline: true,
  emphasized: true,
  importance: #HIGH,
  position: 100,
  invocationGrouping: #CHANGE_SET}]
  @UI.textArrangement: #TEXT_ONLY
  @UI.fieldGroup: [{position: 50, qualifier: 'DefaultInformation' }]
  @EndUserText.label: 'Service Type'
  ServiceType;

  @UI.hidden: true
  @UI.lineItem: [ { position: 70, importance: #HIGH }]
  point_of_origin;

  @EndUserText.label: 'Point of Origin'
  point_of_origin_ver;

  @UI.selectionField: [{ position: 80   }]
//  @UI.hidden: true
  TourId;

  @UI.lineItem: [ { position: 90, importance: #HIGH }]
  @UI.fieldGroup: [{position: 60, qualifier: 'DefaultInformation' }]
  sms_notification;

  @UI.lineItem: [ { position: 100, importance: #HIGH }]
  //  @UI.fieldGroup: [{position: 60, qualifier: 'DefaultInformation' }]
  @UI.selectionField: [{ position: 90 }]
  customer_blocked;
  //@Consumption.filter: { mandatory: false, hidden: false, selectionType: #INTERVAL }
  //RequestedDate;


}
