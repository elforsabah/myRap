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


extend view entity /PLCE/C_PDMNLServiceWR with
{
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC' // Reference your new class
  virtual service_criticality : abap.int1,
          /PLCE/R_PDService._ExtCustom.zzpoint_origin_wdoi as point_of_origin,
          /PLCE/R_PDService._ExtCustom.zzsms_notification  as sms_notification,
          /PLCE/R_PDService._ExtCustom.zzcustomerblocked   as customer_blocked,

          @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC' // Add for the flag
  virtual point_of_origin_ver : zwr_point_origin_wdoi


          // Value of this field(flag) will come from the table EWA_ORDER_OBJECT(field ZZPOINT_ORIGIN_WDOI ).
          // It defines if a service has been replanned

          //  Similarly to how tours in the Planning Cockpit get a red bar at the start of the line to
          //   indicate that a deviation occurred in the tour, we will implement a bar at the start of the
          //    line of the service to indicate that this service has been replanned. This means that we
          //    extend the services to include a flag whether the service was created from a copied WDOI.
          //    A filter option on this flag will be added to the Planning Cockpit.
}




METHOD if_sadl_exit_calc_element_read~calculate.
  FIELD-SYMBOLS: <ls_original>             TYPE any,
                 <ls_calculated>           TYPE any,
                 <lv_RefId>                TYPE any,
                 <lv_service_criticality>  TYPE int1,
                 <lv_point_of_origin_wdoi> TYPE zwr_point_origin_wdoi.

  CLEAR ct_calculated_data.
  DATA lv_test TYPE String.
  LOOP AT it_original_data ASSIGNING <ls_original>.
    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

    ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_RefId>.
    IF sy-subrc = 0.
      DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.

      " Fetch via compliant CDS (not direct table)
      SELECT SINGLE point_of_origin  " aliased field for ZZPOINT_ORIGIN_WDOI
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @<lv_RefId>
        INTO @lv_zzpoint_origin_wdoi.
      IF sy-subrc = 0.
        " Map to 1 (red/critical if replanned) or 0 (neutral) — fixed reversed logic
        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
        IF sy-subrc = 0.
          <lv_service_criticality> = COND #( WHEN lv_zzpoint_origin_wdoi IS NOT INITIAL THEN 1 ELSE 0 ).
        ENDIF.

        ASSIGN COMPONENT 'POINT_OF_ORIGIN_VER' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin_wdoi>.
        IF sy-subrc = 0.
          data(lv_zzpoint_origin_wdoi_temp) = lv_zzpoint_origin_wdoi+2. "skip the first 'WE'
          SHIFT lv_zzpoint_origin_wdoi_temp LEFT DELETING LEADING '0'.
          <lv_point_of_origin_wdoi> = lv_zzpoint_origin_wdoi_temp.
        ENDIF.
      ELSE.
        " Fallback: Neutral if no data
        IF <lv_service_criticality> IS ASSIGNED.
          <lv_service_criticality> = 0.
        ENDIF.
      ENDIF.
    ENDIF.
  ENDLOOP.
ENDMETHOD.

CLASS lhc_service DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS zzservicecreatedetermination FOR DETERMINE ON MODIFY
      IMPORTING keys FOR service~zzservicecreatedetermination.

    METHODS setpointoforigin FOR DETERMINE ON MODIFY
      IMPORTING keys FOR service~setpointoforigin.
    METHODS setsmsnotification FOR DETERMINE ON MODIFY
      IMPORTING keys FOR service~setsmsnotification.
    METHODS addlongtexts FOR DETERMINE ON MODIFY
      IMPORTING keys FOR service~addlongtexts.
    METHODS setcustomerblock FOR DETERMINE ON MODIFY
      IMPORTING keys FOR service~setcustomerblock.
    METHODS setextblock FOR MODIFY
      IMPORTING keys FOR ACTION service~setextblock.
    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR service RESULT result.

ENDCLASS.

CLASS lhc_service IMPLEMENTATION.

  METHOD setpointoforigin.
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( referenceid serviceuuid ) " // Use CDS casing; add ServiceUUID for key
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services)
      FAILED DATA(lt_failed).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.
      SELECT SINGLE point_of_origin
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @<ls_service>-referenceid
        INTO @lv_zzpoint_origin_wdoi.

      IF sy-subrc = 0 AND lv_zzpoint_origin_wdoi IS NOT INITIAL.
*     " // Check if ExtCustom exists; read it first
        READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY extcustom
          FIELDS ( serviceuuid )  "// Minimal to check existence
          WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
          RESULT DATA(lt_extcustom).

        IF lt_extcustom IS INITIAL.
*        // Create if missing
          DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
                lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.
          APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
          <ls_ext_create>-serviceuuid = <ls_service>-serviceuuid. " // %tky or key
          <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

          MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
            ENTITY service
            CREATE BY \_extcustom
            AUTO FILL CID SET FIELDS WITH lt_ext_create
            MAPPED DATA(lmapped)
            FAILED DATA(lfailed)
            REPORTED DATA(lreported).
*        // Handle errors if needed
        ENDIF.

*      // Now update the field
        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY extcustom
          UPDATE FIELDS ( zzpoint_origin_wdoi )
          WITH VALUE #( ( %key-serviceuuid = <ls_service>-serviceuuid
                          zzpoint_origin_wdoi = lv_zzpoint_origin_wdoi ) )
          FAILED DATA(lt_failed_update)
          REPORTED DATA(lt_reported_update).

        " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
        READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
            ALL FIELDS WITH CORRESPONDING #( keys )
          RESULT DATA(lt_service_result).

        CLEAR: lt_ext_create,  lt_reported_update,  lt_failed_update,  lreported, lfailed, lmapped.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.


  METHOD zzservicecreatedetermination.
    DATA:
      lv_cid            TYPE string VALUE '$abap_cid1_',
      lt_service_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.

    LOOP AT keys REFERENCE INTO DATA(lkey).
      INSERT INITIAL LINE INTO TABLE lt_service_create ASSIGNING FIELD-SYMBOL(<ls_service_create>) ELSE UNASSIGN.
      IF <ls_service_create> IS ASSIGNED.
        <ls_service_create>-%tky = lkey->%tky.
        <ls_service_create>-%target = VALUE #( ( %cid = lv_cid && sy-tabix ) ).
      ENDIF.
    ENDLOOP.


    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
       CREATE BY \_extcustom
         AUTO FILL CID SET FIELDS WITH lt_service_create
       MAPPED DATA(lmapped)
       FAILED DATA(lfailed)
       REPORTED DATA(lreported).

    /plce/cl_base_misc=>check_response( EXPORTING is_response = lreported-%other CHANGING ct_messages = reported-%other ).
  ENDMETHOD.

  METHOD setsmsnotification.
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
       ENTITY service
       ALL FIELDS WITH CORRESPONDING #( keys )
       RESULT DATA(lt_services)
       FAILED DATA(lt_failed).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

      DATA lv_send_sms TYPE zwr_dcmgm_send_sms.

      SELECT SINGLE zzsms_notification
        FROM ewa_order_object
        WHERE pobjnr = @<ls_service>-referenceinternalid
        INTO @lv_send_sms.

      IF lv_send_sms IS INITIAL. CONTINUE. ENDIF.

*     " // Check if ExtCustom exists; read it first
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service BY \_extcustom
        ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.

*        // Create if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
              lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.

        APPEND VALUE #(
          %tky = <ls_service>-%tky
          %target = VALUE #(
            ( %cid = lv_cid
              zzsms_notification = lv_send_sms )
          )
        ) TO lt_ext_create.

        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
          CREATE BY \_extcustom
          FROM lt_ext_create
          MAPPED   DATA(lt_mapped_create)
          FAILED   DATA(lt_failed_create)
          REPORTED DATA(lt_reported_create).

      ELSE.

        DATA lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdserviceextcustom.

        "Find the existing row for this service (example: first match)
        READ TABLE lt_extcustom ASSIGNING FIELD-SYMBOL(<ls_ext>)
                                WITH TABLE KEY id COMPONENTS %tky = <ls_service>-%tky.

        IF <ls_ext> IS ASSIGNED.

          APPEND VALUE #(
            %tky = <ls_ext>-%tky
            zzsms_notification = lv_send_sms
          ) TO lt_ext_update.

          MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
            ENTITY extcustom
            UPDATE FIELDS ( zzsms_notification )
            WITH lt_ext_update
            FAILED   DATA(lt_failed_update)
            REPORTED DATA(lt_reported_update).

        ENDIF.

      ENDIF.

      " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      CLEAR: lt_ext_create, lt_mapped_create, lt_failed_create, lt_reported_create, lt_ext_update, lt_reported_update, lt_failed_update.

    ENDLOOP.
  ENDMETHOD.

  METHOD addlongtexts.
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
         ENTITY service
         ALL FIELDS WITH CORRESPONDING #( keys )
         RESULT DATA(lt_services)
         FAILED DATA(lt_failed).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA zz_fahrer_notiz  TYPE zwr_dcmgm_fahrer_notiz.
      DATA zz_interne_notiz TYPE zwr_dcmgm_interne_notiz.
      DATA zz_gefabf_notiz  TYPE zwr_dcmgm_gefabf_notiz.

      SELECT SINGLE *
        FROM ewa_order_object
        WHERE pobjnr = @<ls_service>-referenceinternalid
        INTO @DATA(ls_wdoi).

      IF ls_wdoi IS INITIAL.
        CONTINUE.
      ENDIF.

      SELECT *
          FROM ewaelocsd_new AS eloc
          LEFT OUTER JOIN zwr_ltxt_notes AS notes ON notes~elocsd_guid = eloc~zzelocsd_guid
          WHERE vbeln = @ls_wdoi-sdaufnr
          AND posnr = @ls_wdoi-sdposnr
          AND ( ( @ls_wdoi-ibase     IS INITIAL AND equnr = @ls_wdoi-container ) OR
                ( @ls_wdoi-container IS INITIAL AND ibase = @ls_wdoi-ibase ) )
          AND ab  <= @ls_wdoi-order_date
          AND bis >= @ls_wdoi-order_date
          INTO TABLE @DATA(lt_eloc)
          UP TO 1 ROWS.

      IF lt_eloc IS INITIAL.
        CONTINUE.
      ENDIF.

*     " // Check if ExtCustom exists; read it first
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service BY \_extcustom
        ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
*        // Create if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
              lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.

        APPEND VALUE #(
          %tky = <ls_service>-%tky
          %target = VALUE #(
            ( %cid = lv_cid
              zz_fahrer_notiz  = lt_eloc[ 1 ]-notes-zz_fahrer_notiz
              zz_interne_notiz = lt_eloc[ 1 ]-notes-zz_interne_notiz
              zz_gefabf_notiz  = lt_eloc[ 1 ]-notes-zz_gefabf_notiz )
          )
        ) TO lt_ext_create.

        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
          CREATE BY \_extcustom
          FROM lt_ext_create
          MAPPED   DATA(lt_mapped_create)
          FAILED   DATA(lt_failed_create)
          REPORTED DATA(lt_reported_create).

      ELSE.

        DATA lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdserviceextcustom.

        "Find the existing row for this service (example: first match)
        READ TABLE lt_extcustom ASSIGNING FIELD-SYMBOL(<ls_ext>)
                                WITH TABLE KEY id COMPONENTS %tky = <ls_service>-%tky.

        IF <ls_ext> IS ASSIGNED.

          APPEND VALUE #(
            %tky = <ls_ext>-%tky
            zz_fahrer_notiz  = lt_eloc[ 1 ]-notes-zz_fahrer_notiz
            zz_interne_notiz = lt_eloc[ 1 ]-notes-zz_interne_notiz
            zz_gefabf_notiz  = lt_eloc[ 1 ]-notes-zz_gefabf_notiz
          ) TO lt_ext_update.

          MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
            ENTITY extcustom
            UPDATE FIELDS ( zz_fahrer_notiz zz_interne_notiz zz_gefabf_notiz )
            WITH lt_ext_update
            FAILED   DATA(lt_failed_update)
            REPORTED DATA(lt_reported_update).

        ENDIF.

      ENDIF.

      " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).


      CLEAR: lt_ext_create, lt_mapped_create, lt_failed_create, lt_reported_create, lt_ext_update, lt_reported_update, lt_failed_update.

    ENDLOOP.
  ENDMETHOD.

  METHOD setcustomerblock.
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
         ENTITY service
         ALL FIELDS WITH CORRESPONDING #( keys )
         RESULT DATA(lt_services)
         FAILED DATA(lt_failed).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

      SELECT SINGLE kna1~aufsd
        FROM ewa_order_object AS wdoi
        INNER JOIN kna1 ON kna1~kunnr = wdoi~kunnr
        WHERE pobjnr = @<ls_service>-referenceinternalid
        INTO @DATA(lv_aufsd).

      DATA(lv_customer_blocked) = COND #( WHEN lv_aufsd IS NOT INITIAL
                                          THEN abap_true ELSE abap_false ).

*     " // Check if ExtCustom exists; read it first
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service BY \_extcustom
        ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.

*        // Create if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
              lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.

        APPEND VALUE #(
          %tky = <ls_service>-%tky
          %target = VALUE #(
            ( %cid = lv_cid
              zzcustomerblocked = lv_customer_blocked )
          )
        ) TO lt_ext_create.

        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
          CREATE BY \_extcustom
          FROM lt_ext_create
          MAPPED   DATA(lt_mapped_create)
          FAILED   DATA(ls_failed_create)
          REPORTED DATA(ls_reported_create).

      ELSE.

        DATA lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdserviceextcustom.

        "Find the existing row for this service (example: first match)
        READ TABLE lt_extcustom ASSIGNING FIELD-SYMBOL(<ls_ext>)
                                WITH TABLE KEY id COMPONENTS %tky = <ls_service>-%tky.

        IF <ls_ext> IS ASSIGNED.

          APPEND VALUE #(
            %tky = <ls_ext>-%tky
            zzcustomerblocked = lv_customer_blocked
          ) TO lt_ext_update.

          MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
            ENTITY extcustom
            UPDATE FIELDS ( zzcustomerblocked )
            WITH lt_ext_update
            FAILED   DATA(ls_failed_update)
            REPORTED DATA(ls_reported_update).

        ENDIF.

      ENDIF.

      " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      CLEAR: lt_ext_create, lt_mapped_create, ls_failed_create, ls_reported_create, lt_ext_update, ls_reported_update, ls_failed_update.

    ENDLOOP.
  ENDMETHOD.

  METHOD setextblock.

    DATA lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdserviceextcustom.

    LOOP AT keys INTO DATA(key).

      " Prepare create structure from action parameters
      APPEND VALUE #( %tky-serviceuuid = key-%param-service_uuid
                     zzcustomerblocked = key-%param-zzcustomerblocked )
      TO lt_ext_update.

    ENDLOOP.

    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        UPDATE FIELDS ( zzcustomerblocked )
        WITH lt_ext_update
        FAILED   DATA(ls_failed_update)
        REPORTED DATA(ls_reported_update).

  ENDMETHOD.

  METHOD get_global_authorizations.

    "temporary implementation
    result-%action-setextblock = if_abap_behv=>auth-allowed.

  ENDMETHOD.

ENDCLASS.
