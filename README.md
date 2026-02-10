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
