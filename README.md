Bei Bestätigung des Dialogs erfolgt das Storno:
Falls eingeplant, wird der Service in P&D wird aus seiner Tour entfernt. Das entfällt natürlich, falls ein unverplanter Service storniert wird.
Der Service wird aus der Planung entfernt, sodass keine Karteileiche im P&D verbleibt.
Der Storno-Grund (Freitext) wird als Bemerkung zur zugehörigen Entsorgungsauftragsposition (Feld EWA_ORDER_OBJECT-TEXT) übernommen.
Es wird zudem ein fester Rückmeldehinweis (Feld EWA_ORDER_OBJECT-CONFTYPE1) "Storno durch Dispo"* an der EAP gesetzt, sodass ergänzend zum angegebenen Storno-Grund nachvollziehbar ist, dass das Storno auf diesem Weg erfolgt ist.
(* = diesen Rückmeldehinweis werde noch ich in Rücksprache mit Anja im Customizing definieren.)
Die EAP wird negativ zurückgemeldet.
Diese Storno-Verarbeitung wird später in gleicher Weise anderen Stellen auch benötigt (Interaction Center für Kundenservice und evtl. EWAWDOC für Rückmeldung). Wäre gut, wenn wir das Coding dann weiterverwenden/erweitern können.



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


class lhc_r_pdservice definition inheriting from cl_abap_behavior_handler.
  private section.

    methods get_global_authorizations for global authorization
      importing request requested_authorizations for service result result.
    methods assigntour for modify
      importing keys for action service~assigntour result result.
    methods createtour for modify
      importing keys for action service~createtour result result.
    methods get_instance_features for instance features
      importing keys request requested_features for service result result.
    methods calculateserviceid for determine on modify
      importing keys for service~calculateserviceid.
*    methods planautomatically for modify
*      importing keys for action service~planautomatically.

    methods precheck_createtour for precheck
      importing keys for action service~createtour.
    methods precheck_assigntour for precheck
      importing keys for action service~assigntour.
    methods completeservice for modify
      importing keys for action service~completeservice.
    methods confirmservice for modify
      importing keys for action service~confirmservice.
    methods trycompleteservice for modify
      importing keys for action service~trycompleteservice.
    methods unassignondelete for determine on modify
      importing keys for service~unassignondelete.
    methods validatetasks for validate on save
      importing keys for service~validatetasks.
    methods setinitialstatus for determine on modify
      importing keys for service~setinitialstatus.
    methods calc_changed_stamp for determine on save
      importing keys for service~calc_changed_stamp.

    types: __failed_service   type response for failed early /plce/r_pdservice,
           __reported_service type response for reported early /plce/r_pdservice,
           __service_keys     type table for key of /plce/r_pdservice,
           __Profiles         type standard table of /plce/pdprofile.

    methods check_service_compatibilities
      importing
        keys          type __service_keys
        i_mintourdate type d
        i_maxtourdate type d
*        i_profile     type /plce/profile optional
        i_tourid      type /plce/pdtour_id
        it_profiles   type __Profiles
      changing
        failed        type __failed_service
        reported      type __reported_service.

endclass.

class lhc_r_pdservice implementation.

  method get_global_authorizations.
  endmethod.


  method precheck_assigntour.
    data:
      lprofiles type __profiles.

    read entities of /PLCE/R_PDService in local mode
      entity service fields ( ServiceId )
        with corresponding #( keys )
        result data(services).

    read entities of /PLCE/R_PDTour
      entity Tour
*        fields ( TourId StartDate EndDate PDProfile )
        fields ( TourId StartDate EndDate TourStatus )
        with value #( for k in keys ( TourUUID = k-%param-TourUUID ) )
        result data(tours)
        by \_Profiles all fields with value #( for k in keys ( TourUUID = k-%param-TourUUID ) )
        result data(tour_profiles).

    " check if tour was entered
    failed-service = value #( for k in keys where ( %param-TourUUID is initial ) ( %tky = k-%tky ) ).

    reported-service = value #( for k in keys where ( %param-TourUUID is initial )
    (
      %tky = k-%tky
      %action-assigntour = if_abap_behv=>mk-on
      %msg               = new /plce/cx_pd_exception(
                                                 severity  = if_abap_behv_message=>severity-error
                                                 textid    = /plce/cx_pd_exception=>cs_pdserviceassgtouremptytour
                                                 serviceid = services[ key id %tky  = k-%tky ]-ServiceId )
    ) ).

    if failed is not initial.
      return.
    endif.

    loop at keys assigning field-symbol(<k>) group by <k>-%param-TourUUID assigning field-symbol(<group_tour>).
      " check if tour is completed or confirmed
      if line_exists( tours[ key entity TourUUID = <group_tour> TourStatus = /plce/if_pd_constants=>c_pd_tour_status-completed ] )
      or line_exists( tours[ key entity TourUUID = <group_tour> TourStatus = /plce/if_pd_constants=>c_pd_tour_status-confirmed ] ).
        loop at group <group_tour> assigning field-symbol(<key>).
          data(tour_id) = tours[ key entity TourUUID = <group_tour> ]-TourId.
          append value #( %tky = <key>-%tky
                          %msg = new /plce/cx_pd_exception(
                                   severity = if_abap_behv_message=>severity-error
                                   textid = /plce/cx_pd_exception=>cs_srvc_assgmnt_tour_completed
                                   tour = tour_id
                                 )
                        ) to reported-service.
          append value #( %tky = <key>-%tky ) to failed-service.
          exit.
        endloop.
      endif.

      " check tour constraints
      clear:
        lprofiles.

      data(tour) = tours[ key entity TourUUID = <group_tour> ].
      lprofiles = value #( for pr in tour_profiles using key entity where ( TourUuid = tour-TourUUID ) ( pr-%data-Profile ) ).

      data(mintourdate) = tour-StartDate.
      data(maxtourdate) = cond #( when tour-EndDate is not initial and tour-EndDate > tour-StartDate then tour-EndDate else tour-StartDate ).

      check_service_compatibilities(
       exporting
         keys = value #( for group in group <group_tour> ( group-%key ) )
*         i_profile = tour-PDProfile
         it_profiles = lprofiles
         i_mintourdate = mintourdate
         i_maxtourdate = maxtourdate
         i_tourid = tour-TourId
       changing
         failed = failed
         reported = reported ).
    endloop.

  endmethod.


  method check_service_compatibilities.
    data(has_multiselect_enabled) = /plce/cl_base_misc=>get_capability( /plce/if_constants=>c_backend_capability-stricthandling ).

    "TODO check if service fits to template restrictions /allowed ContainerType etc.
    read entities of /PLCE/R_PDService in local mode
        entity service fields ( ServiceId RequestedDate EarliestDate LatestDate Profile )
          with corresponding #( keys )
          result data(services) ##EML_IN_LOOP_OK.

    loop at services assigning field-symbol(<service>).
      "check profile
*      if <service>-Profile <> i_profile.
      read table it_profiles transporting no fields with key table_line = <service>-Profile.
      if sy-subrc is not initial.
        failed-service = value #( base failed-service ( %tky = <service>-%tky ) ).
        reported-service = value #( base reported-service
           (
              %tky = <service>-%tky
              %action-assigntour = if_abap_behv=>mk-on
              %msg               =  new /plce/cx_pd_exception(
                                                   severity  = if_abap_behv_message=>severity-error
                                                   textid    = /plce/cx_pd_exception=>service_tour_profile_mismatch
                                                   serviceid = <service>-ServiceId
                                                   tour = i_tourid
                                                   )
            ) ).
      endif.

      " check if service dates fit to the range of tour
      data(minservicedate) = cond #( when <service>-EarliestDate is not initial then <service>-EarliestDate else '00000000' ).
      data(maxservicedate) = cond #( when <service>-LatestDate is not initial then <service>-LatestDate else '99991231' ).

      if not ( maxservicedate >= i_mintourdate and minservicedate <= i_maxtourdate ).
        "error
        if not line_exists( failed-service[ key id %tky = <service>-%tky ] ).
          failed-service = value #( base failed-service ( %tky = <service>-%tky ) ).
        endif.
        reported-service = value #( base reported-service
         (
            %tky = <service>-%tky
            %action-assigntour = if_abap_behv=>mk-on
            %msg               =  new /plce/cx_pd_exception(
                                                 severity  = if_abap_behv_message=>severity-error
                                                 textid    = /plce/cx_pd_exception=>service_date_outofrange
                                                 serviceid = <service>-serviceid )
          ) ).
      elseif not ( <service>-RequestedDate >= i_mintourdate and <service>-RequestedDate <= i_maxtourdate ).

        if ( lines( services ) = 1 or has_multiselect_enabled is not initial ).
          "warning TODO %action mit assigntour oder createtour füllen
          reported-service = value #( base reported-service
           (
              %tky = <service>-%tky
              "%state_area = 'WarningService'
              "%element-serviceuuid = if_abap_behv=>mk-on
              %action-assigntour = if_abap_behv=>mk-on
              %msg               =  new /plce/cx_pd_exception(
                                                   severity  = if_abap_behv_message=>severity-warning
                                                   textid    = /plce/cx_pd_exception=>service_requested_outofrange
                                                   serviceid = services[ key id %tky  = <service>-%tky ]-ServiceId )
            ) ).
            "append value #( %tky = <service>-%key ) to failed-service.
        endif.
      endif.


    endloop.
  endmethod.


  method precheck_createtour.
    data:
      lprofiles type __profiles.

    read entities of /PLCE/R_PDService in local mode
      entity service fields ( ServiceId RequestedDate earliestdate LatestDate Profile )
        with corresponding #( keys )
        result data(services).

    read entities of /PLCE/R_PDTourTemplate
      entity PDTourTemplate
        fields ( EarliestStartTime MaximumTourDuration MaximumTourDurationUnit )
        with value #( for k in keys ( tourtemplate = k-%param-tour_template ) )
        result data(tourtemplates)
      by \_Profiles all fields with value #( for k in keys ( tourtemplate = k-%param-tour_template ) )
        result data(tour_profiles).


    "check if template is filled
    failed-service = value #( for k in keys where ( %param-tour_template is initial ) ( %tky = k-%tky ) ).

    reported-service = value #( for k in keys where ( %param-tour_template is initial )
    (
      %tky = k-%tky
      %action-assigntour = if_abap_behv=>mk-on
      %msg               = new /plce/cx_pd_exception(
                                                 severity  = if_abap_behv_message=>severity-error
                                                 textid    = /plce/cx_pd_exception=>cs_pdserviceassgtouremptytour
                                                 serviceid = services[ key id %tky  = k-%tky ]-ServiceId )
    ) ).
    if failed is not initial.
      return.
    endif.

    loop at keys assigning field-symbol(<k>) group by ( template = <k>-%param-tour_template tourdate = <k>-%param-start_date ) assigning field-symbol(<group_tourtemplate>).
      clear:
        lprofiles.

      data(tourtemplate) = tourtemplates[ key entity tourtemplate = <group_tourtemplate>-template ].
      lprofiles = value #( for pr in tour_profiles using key entity where ( TourTemplate = tourtemplate-TourTemplate ) ( pr-Profile ) ).

      check_service_compatibilities(
       exporting
         keys = value #( for group in group <group_tourtemplate> ( group-%key ) )
*         i_profile = tourtemplate-PDProfile
         it_profiles = lprofiles
         i_mintourdate = <group_tourtemplate>-tourdate
         i_maxtourdate = <group_tourtemplate>-tourdate
         i_tourid = conv #( tourtemplate-tourtemplate )
       changing
         failed = failed
         reported = reported ).
    endloop.

  endmethod.

  method assigntour.

    read entities of /PLCE/R_PDTour entity Tour
    fields ( TourId ) with value #( for k in keys ( TourUUID = k-%param-TourUUID  ) )
    result data(tours).

    " call tour->assign services per tour (grouped)
    loop at keys assigning field-symbol(<key>) group by ( touruuid = <key>-%param-TourUUID sequence = <key>-%param-Sequence placebefore = <key>-%param-PlaceBefore  )  assigning field-symbol(<tour_group>).
      modify entities of /PLCE/R_PDTour
       entity Tour
         execute AssignServices from value #(
         (
          TourUUID = <tour_group>-touruuid
          %param = value #(
            sequence = <tour_group>-sequence
            placebefore = <tour_group>-placebefore
            _services =  value #( for s in group <tour_group> ( s-ServiceUUID ) )
          ) ) )
         failed data(failed_tour)
         reported data(reported_tour) .

      if failed_tour is not initial.
        exit.
      endif.

      append new  /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>service_assigned
                                            severity = if_abap_behv_message=>severity-success
                                            tour = tours[ key entity touruuid = <tour_group>-touruuid ]-tourid  ) to reported-%other.
    endloop.

    if failed_tour is not initial.
      clear reported-%other.
      failed-service = value #( for k in keys ( %tky = k-%tky ) ).
      /plce/cl_pd_messagehelper=>collect_reported_tour( exporting messages = reported_tour changing result = reported-%other ).
    endif.

    " results
    read entities of /PLCE/R_PDService in local mode
      entity Service
      all fields with corresponding #( keys )
      result data(service_result).

    result = value #( for service  in service_result ( %tky = service-%tky %param = service ) ).
  endmethod.

  method createtour.
    " call tour->assign services per tour (grouped)
    loop at keys assigning field-symbol(<key>) group by (  template = <key>-%param-tour_template date = <key>-%param-start_date ) assigning field-symbol(<group>).

      "create new tour from template
      modify entities of /PLCE/R_PDTour
        entity Tour
          execute createTourWithTemplate auto fill cid
          with value #( ( %param-without_draft = 'X' %param-tour_template =  <group>-template %param-start_date = <group>-date ) )
            mapped data(mapped_tour)
            failed data(failed_tour)
            reported data(reported_tour).

      "assign services to new created tour
      if failed_tour is initial.
*      loop at group <GROUP> assigning field-symbol(<SRVGRP>) group by ( TEMPLATE = <KEY>-%PARAM-TOUR_TEMPLATE DATE = <KEY>-%PARAM-START_DATE SEQUENCE = <KEY>-%PARAM-Sequence ) assigning field-symbol(<SEQGROUP>).
        loop at group <group> assigning field-symbol(<srvgrp>) group by ( template = <srvgrp>-%param-tour_template date = <srvgrp>-%param-start_date sequence = <srvgrp>-%param-Sequence ) assigning field-symbol(<seqgroup>).
          modify entities of /PLCE/R_PDTour entity Tour
            execute AssignServices from value #( (
              %tky = mapped_tour-tour[ 1 ]-%tky
              %param = value #( _services =  value #( for s in group <seqgroup> ( s-ServiceUUID ) )
                                Sequence = <seqgroup>-sequence )
          ) )
         failed failed_tour
         reported reported_tour .
        endloop.
      endif.

      if failed_tour is initial.
        read entities of /PLCE/R_PDTour entity Tour
            fields ( TourId ) with corresponding #( mapped_tour-tour )
            result data(tours).

        insert new  /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>tour_created_service_assigned
                                           severity = if_abap_behv_message=>severity-success
                                           tour = tours[ 1 ]-TourId ) into table reported-%other.
      endif.
    endloop.

    if failed_tour is not initial.
      failed-service = value #( for k in keys ( %tky = k-%tky ) ).
      /plce/cl_pd_messagehelper=>collect_reported_tour( exporting messages = reported_tour changing result = reported-%other ).
    endif.

    " results
    read entities of /PLCE/R_PDService in local mode
      entity Service
      all fields with corresponding #( keys )
      result data(service_result).

    result = value #( for service  in service_result ( %tky = service-%tky %param = service ) ).
  endmethod.

  method get_instance_features.
    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceStatus ) with corresponding #( keys )
        result data(services).

     result =
      value #( FOR service IN services
                    LET enable_by_status = COND #( WHEN service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed
                                                     OR service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed
                                                   THEN if_abap_behv=>fc-o-disabled
                                                   ELSE if_abap_behv=>fc-o-enabled )
                    IN ( %tky                = service-%tky
                         %delete             = enable_by_status
                         %action-Edit        = enable_by_status
                         %action-assignTour  = enable_by_status
                         %action-createTour  = enable_by_status
                         %action-confirmService = cond #( when service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed then if_abap_behv=>fc-o-enabled else if_abap_behv=>fc-o-disabled )
                         %assoc-_ServiceTask = enable_by_status )
             ).
  endmethod.

  method calculateserviceid.
    read entities of /PLCE/R_PDService in local mode
      entity Service
        all fields with corresponding #( keys )
      result data(lt_services).

    delete lt_services where serviceid is not initial.
    if lines( lt_services ) > 0.
      try.
          data(lnumber) = /plce/cl_base_misc=>get_next_numbers(
            iv_nr_range_nr = '01'
            iv_object      = '/PLCE/PDS'
            iv_count       = lines( lt_services )
          ).
          if lnumber is not initial.
            modify entities of /plce/r_pdservice in local mode
              entity service
                update
                  from value #( for <key> in lt_services index into i
                  let lid = conv /plce/pdservice_id( lnumber + i ) in
                  (
                    %tky        = <key>-%tky
                    serviceid   = |{ lid alpha = in }|
                    %control-serviceid = if_abap_behv=>mk-on
                   ) )
              reported data(update_reported).

            reported = corresponding #( deep update_reported ).
          else.
            raise exception type /plce/cx_baseexception
              exporting
                textid      = /plce/cx_baseexception=>no_number_received
                attribute_1 = '/PLCE/PDS'
                attribute_2 = '01'.
          endif.
        catch /plce/cx_baseexception into data(lex).
          insert lex into table reported-%other.
      endtry.
    endif.
  endmethod.



*  method planautomatically.
*    data: ls_service type structure for action import /plce/r_pdsplngscenario~createscenariofromservice.
*
*    loop at keys assigning field-symbol(<key>).
**      LS_SERVICE-%PARAM-%DATA = corresponding #( <KEY>-%PARAM ).
**      LS_SERVICE-%PARAM-%DATA-Dummy = '1'.
*      ls_service-%param-%data-scenariodescription = <key>-%param-scenariodescription.
*      ls_service-%param-%data-datefrom = <key>-%param-datefrom.
*      ls_service-%param-%data-dateto = <key>-%param-dateto.
**      LS_SERVICE-%PARAM-%DATA-TourWorkload = <KEY>-%PARAM-TourWorkload.
**      LS_SERVICE-%PARAM-%DATA-TourBufferTime = <KEY>-%PARAM-TourBufferTime.
**      LS_SERVICE-%PARAM-%DATA-TourBufferTimeUnit = <KEY>-%PARAM-TourBufferTimeUnit.
**      LS_SERVICE-%PARAM-%DATA-TourConflictHandling = <KEY>-%PARAM-TourConflictHandling.
*      ls_service-%param-%data-releasedirectly = <key>-%param-releasedirectly.
**      if ls_service-%param-%data-Language is initial.
**        ls_service-%param-%data-Language = sy-langu.
**      endif.
*      insert <key>-%tky-serviceuuid into table ls_service-%param-%data-_pdsservices.
*    endloop.
*
*    modify entities of /plce/r_pdsplngscenario entity planningscenario
*        execute createscenariofromservice auto fill cid with value #( ( ls_service ) )
*        failed data(create_scenario_failed)
*        reported data(create_scenario_reported).
*
*    reported = corresponding #( deep create_scenario_reported ).
**    FAILED   = corresponding #( deep CREATE_SCENARIO_FAILED ).
*  endmethod.

*  method deleteServices.
*    read entities of /PLCE/R_PDSPlngScenario
*      entity PDSPlanningScenario by \_PDSPlanningServiceRef
*        all fields with corresponding #( KEYS )
*      result data(LT_RESULT).
*
*  endmethod.

  method completeService.
    data:
      service_processor type ref to /plce/cl_bgp_pdservice,
      lbadi             type ref to /plce/pd_confirmation,
      ltbapiret         type bapirettab,
      lkeys             type /plce/ppdsrv_key,
      lqueues           type /plce/ppd_bgp_service_queue.

    "read existing services
    read entities of /PLCE/R_PDService in local mode
      entity Service fields ( ServiceUUID Profile )
      with corresponding #( keys )
      result final(services).

    "change status
    modify entities of /PLCE/R_PDService in local mode
      entity Service
        update fields ( ServiceStatus ) with value #( for s in services ( %tky = s-%tky ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed ) )
        create by  \_StatusHistory auto fill cid
          set fields with value #( for s in services
            ( %tky = s-%tky
              %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed ) ) ) )
      mapped mapped
      failed failed
      reported reported.

    if failed is initial.
*** confirm service automatically
*      create object service_processor.
*      service_processor->service_confirm = corresponding #(  keys ).
*      /plce/cl_bgprocessing_helper=>schedule(
*        i_bgunit = service_processor
*        i_associated_entities = value #( for k in keys ( EntityType = /plce/if_pd_constants=>c_pd_entity_semantic_contexts-service EntityUUID = k-ServiceUUID ) )
*      ).
      try.
          loop at services reference into data(lservicegroup) group by lservicegroup->Profile reference into data(lserviceprofile).

            clear:
              lkeys, ltbapiret, lqueues.
*** get queues for bgrfc
            get badi lbadi filters profile = lserviceprofile->*.
            lkeys = value /plce/ppdsrv_key( for l in services where ( Profile = lserviceprofile->* ) ( service_uuid = l-ServiceUUID ) ).

            call badi lbadi->get_service_queues
              exporting
                iv_profile = lserviceprofile->*
                it_keys    = lkeys
              changing
                ct_queues  = lqueues
                ct_bapIRET = ltbapiret.

            /plce/cl_base_misc=>check_bapiret(
              exporting
                it_bapiret  = ltbapiret
              changing
                cs_reported = reported
              ).

            try.
                loop at lqueues reference into data(lqueueref).
                  create object service_processor.
                  service_processor->service_confirm = corresponding #( lqueueref->keys mapping ServiceUUID = SERVICE_UUID ).
                  /plce/cl_bgprocessing_helper=>schedule(
                    i_bgunit = service_processor
                    i_queues = lqueueref->queues
                    i_associated_entities = value #( for k in lqueueref->keys ( EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = k-service_uuid ) )
                  ).
                endloop.
              catch /plce/cx_bgp into data(lex_bgp).
                reported-service = value #( for r in service_processor->service_confirm ( %key = corresponding #( r-%key ) %action-confirmService = if_abap_behv=>mk-on %msg = lex_bgp ) ) .
            endtry.
          endloop.
        catch /plce/cx_baseexception into data(lex).
*          insert lex into table reported-%other.
*          failed-service = corresponding #(  base ( failed-service ) services ).
          failed-service = value #( for e in keys ( %key = corresponding #( e-%key ) %action-completeService = if_abap_behv=>mk-on ) ) .
          reported-service = value #( for e in keys ( %key = corresponding #( e-%key ) %action-completeService = if_abap_behv=>mk-on %msg = lex ) ) .
      endtry.
    endif.
  endmethod.

  method confirmService.
    data:
      lbadi     type ref to /plce/pd_confirmation,
      ltbapiret type bapirettab,
      lkeys     type /plce/ppdsrv_key.

    "read existing services
    read entities of /PLCE/R_PDService in local mode
      entity Service fields ( ServiceUUID Profile )
        with corresponding #( keys )
      result final(services).

* provide keys for every profile
    try.
        loop at services reference into data(lservicegroup) group by lservicegroup->Profile reference into data(lserviceprofile).

          clear:
            lkeys, ltbapiret.

          get badi lbadi filters profile = lserviceprofile->*.
          lkeys = value /plce/ppdsrv_key( for l in services where ( Profile = lserviceprofile->* ) ( service_uuid = l-ServiceUUID ) ).

          call badi lbadi->confirm_services
            exporting
              iv_profile = lserviceprofile->*
              it_keys    = lkeys
            changing
              ct_bapiret = ltbapiret.

          /plce/cl_base_misc=>check_bapiret(
            exporting
              it_bapiret  = ltbapiret
            changing
              cs_reported = reported
          ).

        endloop.

          "change status
        modify entities of /PLCE/R_PDService in local mode
          entity Service
            update fields ( ServiceStatus ) with value #( for s in services
                ( %tky = s-%tky ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed ) )
            create by  \_StatusHistory auto fill cid
              set fields with value #( for s in services
                ( %tky = s-%tky
                  %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed ) ) ) )
          mapped mapped
          failed failed
          reported reported.

      catch /plce/cx_baseexception into data(lex).
        insert lex into table reported-%other.
        failed-service = corresponding #(  base ( failed-service ) services ).
    endtry.


  endmethod.

  method TryCompleteService.
    data: service_processor    type ref to /plce/cl_bgp_pdservice.

    "get involved tours
    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceStatus )
          with corresponding #( keys )
          result data(services)
        by \_ServiceTask fields ( TourUUID )
          with corresponding #( keys )
          result data(servicetasks).

    "get all involved resources
    read entities of /PLCE/R_PDTour
      entity Tour
        fields ( TourUUID TourStatus )
          with value #( for st in servicetasks ( %tky-TourUUID = st-TourUUID ) )
          result data(tours)
        by \_Resources fields ( ResourceId )
          with value #( for st in servicetasks ( %tky-TourUUID = st-TourUUID ) )
          result data(tour_resources).

    "get all available work status
    read entities of /PLCE/R_PDMTour
      entity WorkStatus all fields
        with value #( for t in tours
                      for r in tour_resources using key entity where ( TourUUID = t-TourUUID )
                      for s in servicetasks where ( TourUUID = t-TourUUID )
                        ( %tky-TourUUID = t-TourUUID
                          %tky-ResourceId = r-ResourceId
                          %tky-EntityUUID = s-ServiceUUID
                        )
                    )
          result data(workstatuss)
      "...and confirmation notes
      entity Tour by \_ConfirmationNotes all fields
        with value #( for t in tours
                      for r in tour_resources
                        ( %tky-TourUUID = t-TourUUID
                          %tky-ResourceId = r-ResourceId
                        )
                    )
          result data(tour_confirmations_notes).

    read entities of /PLCE/R_PDConfNote_S
          entity ConfNote fields ( ConfirmationNoteType )
            with value #( for c in tour_confirmations_notes ( %tky-ConfirmationNote = c-ConfirmationNote  ) )
            result data(confirmationnotes).

    "ignore non-deviations and confirmed deviations
    loop at tour_confirmations_notes assigning field-symbol(<tour_confirmations_note>).
      if <tour_confirmations_note>-ConfirmedAt is not initial or not line_exists( confirmationnotes[ key entity ConfirmationNote = <tour_confirmations_note>-ConfirmationNote ConfirmationNoteType = 'DEVIATION' ] ).
        delete tour_confirmations_notes.
      endif.
    endloop.

    try.
        loop at services assigning field-symbol(<service>).
          "not completed/confirmed
          check <service>-ServiceStatus <> /plce/if_pd_constants=>c_pd_service_status-completed and <service>-ServiceStatus <> /plce/if_pd_constants=>c_pd_service_status-confirmed.

          "check if at least 1 confirmed, but no started/suspended is available
          check line_exists( workstatuss[ EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID  WorkStatus = 'COMPLETED' ] )
              and not line_exists( workstatuss[ EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID  WorkStatus = 'STARTED' ]  )
              and not line_exists( workstatuss[ EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID  WorkStatus = 'SUSPENDED' ]  ).

          "check unconfirmed deviations
          check not line_exists( tour_confirmations_notes[ ServiceUUID = <service>-ServiceUUID ] ).

          create object service_processor.
          append value #( ServiceUUID = <service>-ServiceUUID ) to service_processor->service_complete.
          /plce/cl_bgprocessing_helper=>schedule(
                i_bgunit = service_processor
                i_associated_entities = value #( ( EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID ) )
              ).

        endloop.
      catch /plce/cx_baseexception into data(lex).
        failed-service = value #( for k in keys ( %key = corresponding #( k-%key ) %action-TryCompleteService = if_abap_behv=>mk-on ) ) .
        reported-service = value #( for k in keys ( %key = corresponding #( k-%key ) %action-TryCompleteService = if_abap_behv=>mk-on %msg = lex ) ) .
    endtry.
  endmethod.

  METHOD UnassignOnDelete.
    data: assigned_services type table for action import /PLCE/R_PDTour\\ServiceAssignment~UnAssign.

    select asgmt~ServiceUUID, asgmt~TourUUID from /PLCE/R_PDTourServiceAsgmt as asgmt
      inner join @keys as keys on asgmt~ServiceUUID = keys~ServiceUUID into corresponding fields of table @assigned_services.

    if assigned_services is not initial.
      modify entities of /PLCE/R_PDTour
        entity ServiceAssignment
          execute UnAssign from assigned_services
        failed data(srvc_failed)
        reported data(srvc_reported).

    /plce/cl_base_misc=>check_response(
      exporting
        is_response = srvc_reported
      changing
        ct_messages = reported-%other
    ).
    endif.
  ENDMETHOD.

  METHOD validateTasks.
    data: lt_service_tasks_sorted type standard table of /Plce/R_PDServicetask with non-unique sorted key sec_key components ServiceUUID.

    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceUUID ServiceId )
          with value #( for key in keys (
                          %tky = corresponding #( key-%tky ) ) )
    result data(lt_services)
    by \_ServiceTask fields ( ServiceUUID )
          with value #( for key in keys (
                          %tky = corresponding #( key-%tky ) ) )
    result data(lt_service_tasks).
    move-corresponding lt_service_tasks to lt_service_tasks_sorted.

    lt_services = filter #( lt_services except in lt_service_tasks_sorted using key sec_key where ServiceUUID = ServiceUUID ).
    loop at lt_services assigning field-symbol(<ls_service>).
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft ) to failed-service.
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft
                      %msg        = new /plce/cx_pd_exception(
                                      severity = if_abap_behv_message=>severity-error
                                      textid = /plce/cx_pd_exception=>CS_PDSrvcWithoutTask
                                      serviceid = <ls_service>-ServiceId
                                    )
                      %create = if_abap_behv=>mk-on
      ) to reported-service.
    endloop.
  ENDMETHOD.

  METHOD setInitialStatus.
    read entities of /PLCE/R_PDService in local mode
      entity Service
      fields ( ServiceStatus  ) with corresponding #( keys )
      result data(lt_services).

    delete lt_services where ServiceStatus is not initial.
    if lt_services is initial.
      return.
    endif.

    modify entities of /PLCE/R_PDService in local mode
      entity Service
        update fields ( ServiceStatus ) with value #( for ls_service in lt_services
             ( %tky          = ls_service-%tky
               ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-in_planning ) )
         create by  \_StatusHistory auto fill cid
           set fields with value #( for ls_service in lt_services
             ( %tky    = ls_service-%tky
               %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-in_planning ) ) ) )
      reported data(lt_ServiceReported).

    Reported = corresponding #( deep lt_ServiceReported ).
  ENDMETHOD.

  METHOD calc_changed_stamp.
    if keys is not initial.
      get time stamp field data(ts).

      select distinct TourUuid from /PLCE/P_PDTourAssignment
       for all entries in @keys
         where ServiceUuid = @keys-serviceuuid
       into table @data(lt_tour).

      if lt_tour is not initial.
        modify entities of /PLCE/R_PDTour entity Tour
          update fields ( ServiceAssignmentChangedAt )
            with value #( for ukey in lt_tour ( TourUUID = ukey-TourUUID ServiceAssignmentChangedAt = ts ) )
          reported data(ltour_reported).

        /plce/cl_base_misc=>check_response(
          exporting
            is_response = ltour_reported
          changing
            ct_messages = reported-%other
        ).
      endif.
    endif.
  ENDMETHOD.

endclass.

class lhc_pdservicetask definition inheriting from cl_abap_behavior_handler.

  private section.
    methods calculateplanningstatus for determine on modify
      importing keys for servicetask~calculateplanningstatus.
    methods calculateservicetaskid for determine on modify
      importing keys for servicetask~calculateservicetaskid.
    methods calculateduration for determine on modify
      importing keys for servicetask~calculateduration.
    methods cleargeoroute for determine on modify "save
      importing keys for servicetask~cleargeoroute.
    methods get_instance_features for instance features
      importing keys request requested_features for servicetask result result.
    methods deactivateondelete for determine on modify
      importing keys for servicetask~deactivateondelete.
    methods validateservicetasks for validate on save
      importing keys for servicetask~validateservicetasks.
    types:
        tservicetaskreadresult type structure for read result /plce/r_pdservice\\servicetask.
endclass.

class lhc_pdservicetask implementation.

  method calculateplanningstatus.
    data: create_history type table for create /PLCE/R_PDService\_StatusHistory.

    read entities of /plce/r_pdservice in local mode
      entity servicetask fields ( touruuid planningstatus )
      with corresponding #( keys )
      result data(servicetasks).

    "remove up-to-date items
    delete servicetasks
      where ( touruuid is initial and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-unplanned )
      or ( touruuid is not initial and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-planned  ).

    "update service tasks
    loop at servicetasks assigning field-symbol(<servicetask>).
      <servicetask>-planningstatus = cond #( when <servicetask>-touruuid is initial then /plce/if_pd_constants=>c_pd_planning_status-unplanned else /plce/if_pd_constants=>c_pd_planning_status-planned ).
    endloop.

    modify entities of  /plce/r_pdservice in local mode
      entity servicetask
      update fields ( planningstatus ) with corresponding #( servicetasks ).

    "read all tasks for affected services
    read entities of /plce/r_pdservice in local mode
      entity service
        by \_servicetask fields ( serviceuuid planningstatus )
          with corresponding #( servicetasks )
          result data(all_servicestasks)
        fields ( planningstatus ) with corresponding #( servicetasks )
          result data(services).

    "check all affected services
    loop at services assigning field-symbol(<service>).
      types _t type table of /plce/pdservice_task_uuid with default key.
      data(iplanned) = lines( value _t( for line in all_servicestasks where ( serviceuuid = <service>-serviceuuid and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-planned ) ( line-servicetaskuuid ) ) ).
      data(iunplanned) = lines( value _t( for line in all_servicestasks where ( serviceuuid = <service>-serviceuuid and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-unplanned ) ( line-servicetaskuuid ) ) ).
      data target_status type /plce/pdplanning_status.
      if  iplanned > 0 and iunplanned > 0.
        target_status = /plce/if_pd_constants=>c_pd_planning_status-partially_planned.
      elseif iplanned = 0.
        target_status = /plce/if_pd_constants=>c_pd_planning_status-unplanned.
      else.
        target_status = /plce/if_pd_constants=>c_pd_planning_status-planned.
      endif.

      if <service>-planningstatus = target_status.
        delete services.
      else.
        <service>-planningstatus = target_status.
        append value #( %tky = <service>-%tky
                        %target = value #( ( PlanningStatus = target_status ) ) ) to create_history.
      endif.
    endloop.

    "update affected/changed services
    modify entities of  /plce/r_pdservice in local mode
      entity service
        update fields ( planningstatus ) with corresponding #( services )
    "create status history
        create by  \_StatusHistory auto fill cid set fields with create_history
      mapped data(lservice_mapped)
      reported data(lservice_reported).

    read entities of /PLCE/R_PDService in local mode
      entity StatusHistory
      fields ( PlanningStatus ) with corresponding #( lservice_mapped-statushistory )
      result data(lservice_histories).

    reported = corresponding #( deep lservice_reported ).
  endmethod.

  method calculateservicetaskid.
    read entities of /plce/r_pdservice in local mode
      entity servicetask
        all fields with corresponding #( keys )
      result data(lt_service_tasks).

    delete lt_service_tasks where servicetaskid is not initial.
    if lines( lt_service_tasks ) > 0.
      try.
          data(lnumber) = /plce/cl_base_misc=>get_next_numbers(
            iv_nr_range_nr = '01'
            iv_object      = '/PLCE/PDST'
            iv_count       = lines( lt_service_tasks )
          ).
          if lnumber is not initial.
            modify entities of /plce/r_pdservice in local mode
              entity servicetask
                update
                  from value #( for <key> in keys index into i
                  let lid = conv /plce/pdservice_task_id( lnumber + i ) in
                  (
                    %tky = <key>-%tky
                    servicetaskid = |{ lid alpha = in }|
                    %control-servicetaskid = if_abap_behv=>mk-on ) )
              reported data(update_reported).

            reported = corresponding #( deep update_reported ).
          else.
            raise exception type /plce/cx_baseexception
              exporting
                textid      = /plce/cx_baseexception=>no_number_received
                attribute_1 = '/PLCE/PDST'
                attribute_2 = '01'.
          endif.
        catch /plce/cx_baseexception into data(lex).
          insert lex into table reported-%other.
      endtry.
    endif.
  endmethod.


  method calculateduration.
    read entities of /PLCE/R_PDService in local mode
      entity ServiceTask
        fields ( TaskType SequenceNumber )
          with corresponding #( keys )
          result data(tasks)
        by \_service fields ( ServiceType Profile )
          with corresponding #( keys )
            result data(services)
            .

    " select involved templates
    select distinct template~servicetype, template~tasktype, template~sqncnr, template~profile, template~duration, template~durationunit
    from /PLCE/R_PDServiceTaskTmpl as template
      inner join @services as services
        on template~profile = services~Profile and template~ServiceType = services~ServiceType
    into table @data(tasktemlates).

    " select all FuncLocCats
    select * from /PLCE/R_MDFuncLocMDCat
    into table @data(funcloccats). "#EC CI_ALL_FIELDS_NEEDED "#EC CI_NOWHERE

    " adjust duration/unit
    loop at tasks assigning field-symbol(<task>).
      case <task>-functionallocationcategory.
        when /plce/if_md_constants=>c_funcloc_mdcategories-customerlocation.
          data(template) = value #( tasktemlates[
              Profile = services[ key entity ServiceUUID = <task>-ServiceUUID ]-Profile
              ServiceType = services[ key entity ServiceUUID = <task>-ServiceUUID ]-ServiceType
              TaskType = <task>-TaskType SqncNr = <task>-SequenceNumber ] optional ).
          <task>-duration = template-duration.
          <task>-durationunit = template-durationunit.

        when /plce/if_md_constants=>c_funcloc_mdcategories-materialdepot or /plce/if_md_constants=>c_funcloc_mdcategories-transportpackagingdepot.
          if <task>-duration is initial or <task>-durationunit is initial.
            data(flc) = value #( funcloccats[ funclocmdcategory = <task>-functionallocationcategory ] optional ).
            if sy-subrc is initial.
              <task>-duration = flc-stayduration.
              <task>-durationunit = flc-staydurationunit.
            endif.
          endif.
        when others.
      endcase.
    endloop.

    "update tasks
    modify entities of /PLCE/R_PDService in local mode
      entity ServiceTask
        update fields ( Duration DurationUnit )
          with value #( for t in tasks ( %tky = t-%tky Duration = t-Duration DurationUnit = t-DurationUnit ) )
      reported data(lservice_reported).

    reported = corresponding #( deep lservice_reported ).
  endmethod.


  method ClearGeoRoute.
    read entities of /PLCE/R_PDService in local mode
      entity ServiceTask fields ( ServiceTaskUUID TourUUID ) with corresponding #( keys )
      result data(lt_servicetask).

    read entities of /PLCE/R_PDTour
      entity Tour fields ( TourUUID ) with corresponding #( lt_servicetask )
      result data(lt_tour).

    if lt_tour is not initial.
      modify entities of /PLCE/R_PDTour
        entity Tour execute DeleteGeoRoute from corresponding #( lt_tour )
        reported data(ltour_reported)
        failed data(ltour_failed).

      /plce/cl_base_misc=>check_response(
        exporting
          is_response = ltour_reported
        changing
          ct_messages = reported-%other
      ).
    endif.
  endmethod.

  method get_instance_features.
    read entities of /PLCE/R_PDService in local mode
      entity ServiceTask
        fields ( ServiceUUID ServiceStatus ) with corresponding #( keys )
        result data(service_tasks).

     result =
        value #( FOR st IN service_tasks
                       ( %tky                = st-%tky
                         %delete             = COND #( WHEN st-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed
                                                         OR st-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed
                                                       THEN if_abap_behv=>fc-o-disabled
                                                       ELSE if_abap_behv=>fc-o-enabled ) )
             ).
  endmethod.

  method DeactivateOnDelete.
    data: assigned_tasks type table for action import /PLCE/R_PDTour\\TaskAssignment~Deactivate.

    select st~ServiceTaskUUID, st~ServiceUUID, st~TourUUID, 'S' as TaskScope from /PLCE/R_PDServiceTask as st
      inner join @keys as keys on st~ServiceTaskUUID = keys~ServiceTaskUUID where TourUUID is not initial into corresponding fields of table @assigned_tasks.

    if assigned_tasks is not initial.
      modify entities of /PLCE/R_PDTour
        entity TaskAssignment
          execute Deactivate from assigned_tasks
        failed data(st_failed)
        reported data(st_reported).

    /plce/cl_base_misc=>check_response(
      exporting
        is_response = st_reported
      changing
        ct_messages = reported-%other
    ).
    endif.
  endmethod.

  METHOD validateServiceTasks.
    data: lt_service_tasks_sorted type standard table of /Plce/R_PDServicetask with non-unique sorted key sec_key components ServiceUUID.

    " read ServiceUUID before commit
    select distinct st~ServiceUUID from /PLCE/R_PDServiceTask as st
      inner join @keys as keys on st~ServiceTaskUUID = keys~ServiceTaskUUID
      into table @data(lt_service_keys).

    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceUUID ServiceId )
          with value #( for key in lt_service_keys (
                          ServiceUUID = key-ServiceUUID ) )
    result data(lt_services)
    by \_ServiceTask fields ( ServiceUUID )
          with value #( for key in lt_service_keys (
                          ServiceUUID = key-ServiceUUID ) )
    result data(lt_service_tasks).
    move-corresponding lt_service_tasks to lt_service_tasks_sorted.

    lt_services = filter #( lt_services except in lt_service_tasks_sorted using key sec_key where ServiceUUID = ServiceUUID ).
    loop at lt_services assigning field-symbol(<ls_service>).
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft ) to failed-servicetask.
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft
                      %msg        = new /plce/cx_pd_exception(
                                      severity = if_abap_behv_message=>severity-error
                                      textid = /plce/cx_pd_exception=>CS_PDSrvcWithoutTask
                                      serviceid = <ls_service>-ServiceId
                                    )
                      %delete = if_abap_behv=>mk-on
      ) to reported-servicetask.
    endloop.
  ENDMETHOD.

endclass.

class lhc_attachments definition inheriting from cl_abap_behavior_handler.

  private section.

    methods validateattachment for validate on save
      importing keys for attachments~validateattachment.

endclass.

class lhc_attachments implementation.
  method validateattachment.
    read entities of /plce/r_pdservice in local mode
      entity attachments
        fields ( attachment )
        with corresponding #( keys )
        result data(lt_service_attachments).

    loop at lt_service_attachments assigning field-symbol(<ls_service_attachment>) where attachment is initial.
      append value #( %key      = corresponding #( <ls_service_attachment>-%key )
                      %is_draft = <ls_service_attachment>-%is_draft ) to failed-service.
      append value #( %key      = corresponding #( <ls_service_attachment>-%key )
                      %is_draft = <ls_service_attachment>-%is_draft
                      %msg        = new /plce/cx_pd_exception(
                                        severity = if_abap_behv_message=>severity-error
                                        textid = /plce/cx_pd_exception=>cs_service_attachment_empty
                                    )
                      %create = if_abap_behv=>mk-on
      ) to reported-service.
    endloop.
  endmethod.
endclass.


class lhc_r_pdservice definition inheriting from cl_abap_behavior_handler.
  private section.

    methods get_global_authorizations for global authorization
      importing request requested_authorizations for service result result.
    methods assigntour for modify
      importing keys for action service~assigntour result result.
    methods createtour for modify
      importing keys for action service~createtour result result.
    methods get_instance_features for instance features
      importing keys request requested_features for service result result.
    methods calculateserviceid for determine on modify
      importing keys for service~calculateserviceid.
*    methods planautomatically for modify
*      importing keys for action service~planautomatically.

    methods precheck_createtour for precheck
      importing keys for action service~createtour.
    methods precheck_assigntour for precheck
      importing keys for action service~assigntour.
    methods completeservice for modify
      importing keys for action service~completeservice.
    methods confirmservice for modify
      importing keys for action service~confirmservice.
    methods trycompleteservice for modify
      importing keys for action service~trycompleteservice.
    methods unassignondelete for determine on modify
      importing keys for service~unassignondelete.
    methods validatetasks for validate on save
      importing keys for service~validatetasks.
    methods setinitialstatus for determine on modify
      importing keys for service~setinitialstatus.
    methods calc_changed_stamp for determine on save
      importing keys for service~calc_changed_stamp.

    types: __failed_service   type response for failed early /plce/r_pdservice,
           __reported_service type response for reported early /plce/r_pdservice,
           __service_keys     type table for key of /plce/r_pdservice,
           __Profiles         type standard table of /plce/pdprofile.

    methods check_service_compatibilities
      importing
        keys          type __service_keys
        i_mintourdate type d
        i_maxtourdate type d
*        i_profile     type /plce/profile optional
        i_tourid      type /plce/pdtour_id
        it_profiles   type __Profiles
      changing
        failed        type __failed_service
        reported      type __reported_service.

endclass.

class lhc_r_pdservice implementation.

  method get_global_authorizations.
  endmethod.


  method precheck_assigntour.
    data:
      lprofiles type __profiles.

    read entities of /PLCE/R_PDService in local mode
      entity service fields ( ServiceId )
        with corresponding #( keys )
        result data(services).

    read entities of /PLCE/R_PDTour
      entity Tour
*        fields ( TourId StartDate EndDate PDProfile )
        fields ( TourId StartDate EndDate TourStatus )
        with value #( for k in keys ( TourUUID = k-%param-TourUUID ) )
        result data(tours)
        by \_Profiles all fields with value #( for k in keys ( TourUUID = k-%param-TourUUID ) )
        result data(tour_profiles).

    " check if tour was entered
    failed-service = value #( for k in keys where ( %param-TourUUID is initial ) ( %tky = k-%tky ) ).

    reported-service = value #( for k in keys where ( %param-TourUUID is initial )
    (
      %tky = k-%tky
      %action-assigntour = if_abap_behv=>mk-on
      %msg               = new /plce/cx_pd_exception(
                                                 severity  = if_abap_behv_message=>severity-error
                                                 textid    = /plce/cx_pd_exception=>cs_pdserviceassgtouremptytour
                                                 serviceid = services[ key id %tky  = k-%tky ]-ServiceId )
    ) ).

    if failed is not initial.
      return.
    endif.

    loop at keys assigning field-symbol(<k>) group by <k>-%param-TourUUID assigning field-symbol(<group_tour>).
      " check if tour is completed or confirmed
      if line_exists( tours[ key entity TourUUID = <group_tour> TourStatus = /plce/if_pd_constants=>c_pd_tour_status-completed ] )
      or line_exists( tours[ key entity TourUUID = <group_tour> TourStatus = /plce/if_pd_constants=>c_pd_tour_status-confirmed ] ).
        loop at group <group_tour> assigning field-symbol(<key>).
          data(tour_id) = tours[ key entity TourUUID = <group_tour> ]-TourId.
          append value #( %tky = <key>-%tky
                          %msg = new /plce/cx_pd_exception(
                                   severity = if_abap_behv_message=>severity-error
                                   textid = /plce/cx_pd_exception=>cs_srvc_assgmnt_tour_completed
                                   tour = tour_id
                                 )
                        ) to reported-service.
          append value #( %tky = <key>-%tky ) to failed-service.
          exit.
        endloop.
      endif.

      " check tour constraints
      clear:
        lprofiles.

      data(tour) = tours[ key entity TourUUID = <group_tour> ].
      lprofiles = value #( for pr in tour_profiles using key entity where ( TourUuid = tour-TourUUID ) ( pr-%data-Profile ) ).

      data(mintourdate) = tour-StartDate.
      data(maxtourdate) = cond #( when tour-EndDate is not initial and tour-EndDate > tour-StartDate then tour-EndDate else tour-StartDate ).

      check_service_compatibilities(
       exporting
         keys = value #( for group in group <group_tour> ( group-%key ) )
*         i_profile = tour-PDProfile
         it_profiles = lprofiles
         i_mintourdate = mintourdate
         i_maxtourdate = maxtourdate
         i_tourid = tour-TourId
       changing
         failed = failed
         reported = reported ).
    endloop.

  endmethod.


  method check_service_compatibilities.
    data(has_multiselect_enabled) = /plce/cl_base_misc=>get_capability( /plce/if_constants=>c_backend_capability-stricthandling ).

    "TODO check if service fits to template restrictions /allowed ContainerType etc.
    read entities of /PLCE/R_PDService in local mode
        entity service fields ( ServiceId RequestedDate EarliestDate LatestDate Profile )
          with corresponding #( keys )
          result data(services) ##EML_IN_LOOP_OK.

    loop at services assigning field-symbol(<service>).
      "check profile
*      if <service>-Profile <> i_profile.
      read table it_profiles transporting no fields with key table_line = <service>-Profile.
      if sy-subrc is not initial.
        failed-service = value #( base failed-service ( %tky = <service>-%tky ) ).
        reported-service = value #( base reported-service
           (
              %tky = <service>-%tky
              %action-assigntour = if_abap_behv=>mk-on
              %msg               =  new /plce/cx_pd_exception(
                                                   severity  = if_abap_behv_message=>severity-error
                                                   textid    = /plce/cx_pd_exception=>service_tour_profile_mismatch
                                                   serviceid = <service>-ServiceId
                                                   tour = i_tourid
                                                   )
            ) ).
      endif.

      " check if service dates fit to the range of tour
      data(minservicedate) = cond #( when <service>-EarliestDate is not initial then <service>-EarliestDate else '00000000' ).
      data(maxservicedate) = cond #( when <service>-LatestDate is not initial then <service>-LatestDate else '99991231' ).

      if not ( maxservicedate >= i_mintourdate and minservicedate <= i_maxtourdate ).
        "error
        if not line_exists( failed-service[ key id %tky = <service>-%tky ] ).
          failed-service = value #( base failed-service ( %tky = <service>-%tky ) ).
        endif.
        reported-service = value #( base reported-service
         (
            %tky = <service>-%tky
            %action-assigntour = if_abap_behv=>mk-on
            %msg               =  new /plce/cx_pd_exception(
                                                 severity  = if_abap_behv_message=>severity-error
                                                 textid    = /plce/cx_pd_exception=>service_date_outofrange
                                                 serviceid = <service>-serviceid )
          ) ).
      elseif not ( <service>-RequestedDate >= i_mintourdate and <service>-RequestedDate <= i_maxtourdate ).

        if ( lines( services ) = 1 or has_multiselect_enabled is not initial ).
          "warning TODO %action mit assigntour oder createtour füllen
          reported-service = value #( base reported-service
           (
              %tky = <service>-%tky
              "%state_area = 'WarningService'
              "%element-serviceuuid = if_abap_behv=>mk-on
              %action-assigntour = if_abap_behv=>mk-on
              %msg               =  new /plce/cx_pd_exception(
                                                   severity  = if_abap_behv_message=>severity-warning
                                                   textid    = /plce/cx_pd_exception=>service_requested_outofrange
                                                   serviceid = services[ key id %tky  = <service>-%tky ]-ServiceId )
            ) ).
            "append value #( %tky = <service>-%key ) to failed-service.
        endif.
      endif.


    endloop.
  endmethod.


  method precheck_createtour.
    data:
      lprofiles type __profiles.

    read entities of /PLCE/R_PDService in local mode
      entity service fields ( ServiceId RequestedDate earliestdate LatestDate Profile )
        with corresponding #( keys )
        result data(services).

    read entities of /PLCE/R_PDTourTemplate
      entity PDTourTemplate
        fields ( EarliestStartTime MaximumTourDuration MaximumTourDurationUnit )
        with value #( for k in keys ( tourtemplate = k-%param-tour_template ) )
        result data(tourtemplates)
      by \_Profiles all fields with value #( for k in keys ( tourtemplate = k-%param-tour_template ) )
        result data(tour_profiles).


    "check if template is filled
    failed-service = value #( for k in keys where ( %param-tour_template is initial ) ( %tky = k-%tky ) ).

    reported-service = value #( for k in keys where ( %param-tour_template is initial )
    (
      %tky = k-%tky
      %action-assigntour = if_abap_behv=>mk-on
      %msg               = new /plce/cx_pd_exception(
                                                 severity  = if_abap_behv_message=>severity-error
                                                 textid    = /plce/cx_pd_exception=>cs_pdserviceassgtouremptytour
                                                 serviceid = services[ key id %tky  = k-%tky ]-ServiceId )
    ) ).
    if failed is not initial.
      return.
    endif.

    loop at keys assigning field-symbol(<k>) group by ( template = <k>-%param-tour_template tourdate = <k>-%param-start_date ) assigning field-symbol(<group_tourtemplate>).
      clear:
        lprofiles.

      data(tourtemplate) = tourtemplates[ key entity tourtemplate = <group_tourtemplate>-template ].
      lprofiles = value #( for pr in tour_profiles using key entity where ( TourTemplate = tourtemplate-TourTemplate ) ( pr-Profile ) ).

      check_service_compatibilities(
       exporting
         keys = value #( for group in group <group_tourtemplate> ( group-%key ) )
*         i_profile = tourtemplate-PDProfile
         it_profiles = lprofiles
         i_mintourdate = <group_tourtemplate>-tourdate
         i_maxtourdate = <group_tourtemplate>-tourdate
         i_tourid = conv #( tourtemplate-tourtemplate )
       changing
         failed = failed
         reported = reported ).
    endloop.

  endmethod.

  method assigntour.

    read entities of /PLCE/R_PDTour entity Tour
    fields ( TourId ) with value #( for k in keys ( TourUUID = k-%param-TourUUID  ) )
    result data(tours).

    " call tour->assign services per tour (grouped)
    loop at keys assigning field-symbol(<key>) group by ( touruuid = <key>-%param-TourUUID sequence = <key>-%param-Sequence placebefore = <key>-%param-PlaceBefore  )  assigning field-symbol(<tour_group>).
      modify entities of /PLCE/R_PDTour
       entity Tour
         execute AssignServices from value #(
         (
          TourUUID = <tour_group>-touruuid
          %param = value #(
            sequence = <tour_group>-sequence
            placebefore = <tour_group>-placebefore
            _services =  value #( for s in group <tour_group> ( s-ServiceUUID ) )
          ) ) )
         failed data(failed_tour)
         reported data(reported_tour) .

      if failed_tour is not initial.
        exit.
      endif.

      append new  /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>service_assigned
                                            severity = if_abap_behv_message=>severity-success
                                            tour = tours[ key entity touruuid = <tour_group>-touruuid ]-tourid  ) to reported-%other.
    endloop.

    if failed_tour is not initial.
      clear reported-%other.
      failed-service = value #( for k in keys ( %tky = k-%tky ) ).
      /plce/cl_pd_messagehelper=>collect_reported_tour( exporting messages = reported_tour changing result = reported-%other ).
    endif.

    " results
    read entities of /PLCE/R_PDService in local mode
      entity Service
      all fields with corresponding #( keys )
      result data(service_result).

    result = value #( for service  in service_result ( %tky = service-%tky %param = service ) ).
  endmethod.

  method createtour.
    " call tour->assign services per tour (grouped)
    loop at keys assigning field-symbol(<key>) group by (  template = <key>-%param-tour_template date = <key>-%param-start_date ) assigning field-symbol(<group>).

      "create new tour from template
      modify entities of /PLCE/R_PDTour
        entity Tour
          execute createTourWithTemplate auto fill cid
          with value #( ( %param-without_draft = 'X' %param-tour_template =  <group>-template %param-start_date = <group>-date ) )
            mapped data(mapped_tour)
            failed data(failed_tour)
            reported data(reported_tour).

      "assign services to new created tour
      if failed_tour is initial.
*      loop at group <GROUP> assigning field-symbol(<SRVGRP>) group by ( TEMPLATE = <KEY>-%PARAM-TOUR_TEMPLATE DATE = <KEY>-%PARAM-START_DATE SEQUENCE = <KEY>-%PARAM-Sequence ) assigning field-symbol(<SEQGROUP>).
        loop at group <group> assigning field-symbol(<srvgrp>) group by ( template = <srvgrp>-%param-tour_template date = <srvgrp>-%param-start_date sequence = <srvgrp>-%param-Sequence ) assigning field-symbol(<seqgroup>).
          modify entities of /PLCE/R_PDTour entity Tour
            execute AssignServices from value #( (
              %tky = mapped_tour-tour[ 1 ]-%tky
              %param = value #( _services =  value #( for s in group <seqgroup> ( s-ServiceUUID ) )
                                Sequence = <seqgroup>-sequence )
          ) )
         failed failed_tour
         reported reported_tour .
        endloop.
      endif.

      if failed_tour is initial.
        read entities of /PLCE/R_PDTour entity Tour
            fields ( TourId ) with corresponding #( mapped_tour-tour )
            result data(tours).

        insert new  /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>tour_created_service_assigned
                                           severity = if_abap_behv_message=>severity-success
                                           tour = tours[ 1 ]-TourId ) into table reported-%other.
      endif.
    endloop.

    if failed_tour is not initial.
      failed-service = value #( for k in keys ( %tky = k-%tky ) ).
      /plce/cl_pd_messagehelper=>collect_reported_tour( exporting messages = reported_tour changing result = reported-%other ).
    endif.

    " results
    read entities of /PLCE/R_PDService in local mode
      entity Service
      all fields with corresponding #( keys )
      result data(service_result).

    result = value #( for service  in service_result ( %tky = service-%tky %param = service ) ).
  endmethod.

  method get_instance_features.
    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceStatus ) with corresponding #( keys )
        result data(services).

     result =
      value #( FOR service IN services
                    LET enable_by_status = COND #( WHEN service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed
                                                     OR service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed
                                                   THEN if_abap_behv=>fc-o-disabled
                                                   ELSE if_abap_behv=>fc-o-enabled )
                    IN ( %tky                = service-%tky
                         %delete             = enable_by_status
                         %action-Edit        = enable_by_status
                         %action-assignTour  = enable_by_status
                         %action-createTour  = enable_by_status
                         %action-confirmService = cond #( when service-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed then if_abap_behv=>fc-o-enabled else if_abap_behv=>fc-o-disabled )
                         %assoc-_ServiceTask = enable_by_status )
             ).
  endmethod.

  method calculateserviceid.
    read entities of /PLCE/R_PDService in local mode
      entity Service
        all fields with corresponding #( keys )
      result data(lt_services).

    delete lt_services where serviceid is not initial.
    if lines( lt_services ) > 0.
      try.
          data(lnumber) = /plce/cl_base_misc=>get_next_numbers(
            iv_nr_range_nr = '01'
            iv_object      = '/PLCE/PDS'
            iv_count       = lines( lt_services )
          ).
          if lnumber is not initial.
            modify entities of /plce/r_pdservice in local mode
              entity service
                update
                  from value #( for <key> in lt_services index into i
                  let lid = conv /plce/pdservice_id( lnumber + i ) in
                  (
                    %tky        = <key>-%tky
                    serviceid   = |{ lid alpha = in }|
                    %control-serviceid = if_abap_behv=>mk-on
                   ) )
              reported data(update_reported).

            reported = corresponding #( deep update_reported ).
          else.
            raise exception type /plce/cx_baseexception
              exporting
                textid      = /plce/cx_baseexception=>no_number_received
                attribute_1 = '/PLCE/PDS'
                attribute_2 = '01'.
          endif.
        catch /plce/cx_baseexception into data(lex).
          insert lex into table reported-%other.
      endtry.
    endif.
  endmethod.



*  method planautomatically.
*    data: ls_service type structure for action import /plce/r_pdsplngscenario~createscenariofromservice.
*
*    loop at keys assigning field-symbol(<key>).
**      LS_SERVICE-%PARAM-%DATA = corresponding #( <KEY>-%PARAM ).
**      LS_SERVICE-%PARAM-%DATA-Dummy = '1'.
*      ls_service-%param-%data-scenariodescription = <key>-%param-scenariodescription.
*      ls_service-%param-%data-datefrom = <key>-%param-datefrom.
*      ls_service-%param-%data-dateto = <key>-%param-dateto.
**      LS_SERVICE-%PARAM-%DATA-TourWorkload = <KEY>-%PARAM-TourWorkload.
**      LS_SERVICE-%PARAM-%DATA-TourBufferTime = <KEY>-%PARAM-TourBufferTime.
**      LS_SERVICE-%PARAM-%DATA-TourBufferTimeUnit = <KEY>-%PARAM-TourBufferTimeUnit.
**      LS_SERVICE-%PARAM-%DATA-TourConflictHandling = <KEY>-%PARAM-TourConflictHandling.
*      ls_service-%param-%data-releasedirectly = <key>-%param-releasedirectly.
**      if ls_service-%param-%data-Language is initial.
**        ls_service-%param-%data-Language = sy-langu.
**      endif.
*      insert <key>-%tky-serviceuuid into table ls_service-%param-%data-_pdsservices.
*    endloop.
*
*    modify entities of /plce/r_pdsplngscenario entity planningscenario
*        execute createscenariofromservice auto fill cid with value #( ( ls_service ) )
*        failed data(create_scenario_failed)
*        reported data(create_scenario_reported).
*
*    reported = corresponding #( deep create_scenario_reported ).
**    FAILED   = corresponding #( deep CREATE_SCENARIO_FAILED ).
*  endmethod.

*  method deleteServices.
*    read entities of /PLCE/R_PDSPlngScenario
*      entity PDSPlanningScenario by \_PDSPlanningServiceRef
*        all fields with corresponding #( KEYS )
*      result data(LT_RESULT).
*
*  endmethod.

  method completeService.
    data:
      service_processor type ref to /plce/cl_bgp_pdservice,
      lbadi             type ref to /plce/pd_confirmation,
      ltbapiret         type bapirettab,
      lkeys             type /plce/ppdsrv_key,
      lqueues           type /plce/ppd_bgp_service_queue.

    "read existing services
    read entities of /PLCE/R_PDService in local mode
      entity Service fields ( ServiceUUID Profile )
      with corresponding #( keys )
      result final(services).

    "change status
    modify entities of /PLCE/R_PDService in local mode
      entity Service
        update fields ( ServiceStatus ) with value #( for s in services ( %tky = s-%tky ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed ) )
        create by  \_StatusHistory auto fill cid
          set fields with value #( for s in services
            ( %tky = s-%tky
              %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed ) ) ) )
      mapped mapped
      failed failed
      reported reported.

    if failed is initial.
*** confirm service automatically
*      create object service_processor.
*      service_processor->service_confirm = corresponding #(  keys ).
*      /plce/cl_bgprocessing_helper=>schedule(
*        i_bgunit = service_processor
*        i_associated_entities = value #( for k in keys ( EntityType = /plce/if_pd_constants=>c_pd_entity_semantic_contexts-service EntityUUID = k-ServiceUUID ) )
*      ).
      try.
          loop at services reference into data(lservicegroup) group by lservicegroup->Profile reference into data(lserviceprofile).

            clear:
              lkeys, ltbapiret, lqueues.
*** get queues for bgrfc
            get badi lbadi filters profile = lserviceprofile->*.
            lkeys = value /plce/ppdsrv_key( for l in services where ( Profile = lserviceprofile->* ) ( service_uuid = l-ServiceUUID ) ).

            call badi lbadi->get_service_queues
              exporting
                iv_profile = lserviceprofile->*
                it_keys    = lkeys
              changing
                ct_queues  = lqueues
                ct_bapIRET = ltbapiret.

            /plce/cl_base_misc=>check_bapiret(
              exporting
                it_bapiret  = ltbapiret
              changing
                cs_reported = reported
              ).

            try.
                loop at lqueues reference into data(lqueueref).
                  create object service_processor.
                  service_processor->service_confirm = corresponding #( lqueueref->keys mapping ServiceUUID = SERVICE_UUID ).
                  /plce/cl_bgprocessing_helper=>schedule(
                    i_bgunit = service_processor
                    i_queues = lqueueref->queues
                    i_associated_entities = value #( for k in lqueueref->keys ( EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = k-service_uuid ) )
                  ).
                endloop.
              catch /plce/cx_bgp into data(lex_bgp).
                reported-service = value #( for r in service_processor->service_confirm ( %key = corresponding #( r-%key ) %action-confirmService = if_abap_behv=>mk-on %msg = lex_bgp ) ) .
            endtry.
          endloop.
        catch /plce/cx_baseexception into data(lex).
*          insert lex into table reported-%other.
*          failed-service = corresponding #(  base ( failed-service ) services ).
          failed-service = value #( for e in keys ( %key = corresponding #( e-%key ) %action-completeService = if_abap_behv=>mk-on ) ) .
          reported-service = value #( for e in keys ( %key = corresponding #( e-%key ) %action-completeService = if_abap_behv=>mk-on %msg = lex ) ) .
      endtry.
    endif.
  endmethod.

  method confirmService.
    data:
      lbadi     type ref to /plce/pd_confirmation,
      ltbapiret type bapirettab,
      lkeys     type /plce/ppdsrv_key.

    "read existing services
    read entities of /PLCE/R_PDService in local mode
      entity Service fields ( ServiceUUID Profile )
        with corresponding #( keys )
      result final(services).

* provide keys for every profile
    try.
        loop at services reference into data(lservicegroup) group by lservicegroup->Profile reference into data(lserviceprofile).

          clear:
            lkeys, ltbapiret.

          get badi lbadi filters profile = lserviceprofile->*.
          lkeys = value /plce/ppdsrv_key( for l in services where ( Profile = lserviceprofile->* ) ( service_uuid = l-ServiceUUID ) ).

          call badi lbadi->confirm_services
            exporting
              iv_profile = lserviceprofile->*
              it_keys    = lkeys
            changing
              ct_bapiret = ltbapiret.

          /plce/cl_base_misc=>check_bapiret(
            exporting
              it_bapiret  = ltbapiret
            changing
              cs_reported = reported
          ).

        endloop.

          "change status
        modify entities of /PLCE/R_PDService in local mode
          entity Service
            update fields ( ServiceStatus ) with value #( for s in services
                ( %tky = s-%tky ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed ) )
            create by  \_StatusHistory auto fill cid
              set fields with value #( for s in services
                ( %tky = s-%tky
                  %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed ) ) ) )
          mapped mapped
          failed failed
          reported reported.

      catch /plce/cx_baseexception into data(lex).
        insert lex into table reported-%other.
        failed-service = corresponding #(  base ( failed-service ) services ).
    endtry.


  endmethod.

  method TryCompleteService.
    data: service_processor    type ref to /plce/cl_bgp_pdservice.

    "get involved tours
    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceStatus )
          with corresponding #( keys )
          result data(services)
        by \_ServiceTask fields ( TourUUID )
          with corresponding #( keys )
          result data(servicetasks).

    "get all involved resources
    read entities of /PLCE/R_PDTour
      entity Tour
        fields ( TourUUID TourStatus )
          with value #( for st in servicetasks ( %tky-TourUUID = st-TourUUID ) )
          result data(tours)
        by \_Resources fields ( ResourceId )
          with value #( for st in servicetasks ( %tky-TourUUID = st-TourUUID ) )
          result data(tour_resources).

    "get all available work status
    read entities of /PLCE/R_PDMTour
      entity WorkStatus all fields
        with value #( for t in tours
                      for r in tour_resources using key entity where ( TourUUID = t-TourUUID )
                      for s in servicetasks where ( TourUUID = t-TourUUID )
                        ( %tky-TourUUID = t-TourUUID
                          %tky-ResourceId = r-ResourceId
                          %tky-EntityUUID = s-ServiceUUID
                        )
                    )
          result data(workstatuss)
      "...and confirmation notes
      entity Tour by \_ConfirmationNotes all fields
        with value #( for t in tours
                      for r in tour_resources
                        ( %tky-TourUUID = t-TourUUID
                          %tky-ResourceId = r-ResourceId
                        )
                    )
          result data(tour_confirmations_notes).

    read entities of /PLCE/R_PDConfNote_S
          entity ConfNote fields ( ConfirmationNoteType )
            with value #( for c in tour_confirmations_notes ( %tky-ConfirmationNote = c-ConfirmationNote  ) )
            result data(confirmationnotes).

    "ignore non-deviations and confirmed deviations
    loop at tour_confirmations_notes assigning field-symbol(<tour_confirmations_note>).
      if <tour_confirmations_note>-ConfirmedAt is not initial or not line_exists( confirmationnotes[ key entity ConfirmationNote = <tour_confirmations_note>-ConfirmationNote ConfirmationNoteType = 'DEVIATION' ] ).
        delete tour_confirmations_notes.
      endif.
    endloop.

    try.
        loop at services assigning field-symbol(<service>).
          "not completed/confirmed
          check <service>-ServiceStatus <> /plce/if_pd_constants=>c_pd_service_status-completed and <service>-ServiceStatus <> /plce/if_pd_constants=>c_pd_service_status-confirmed.

          "check if at least 1 confirmed, but no started/suspended is available
          check line_exists( workstatuss[ EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID  WorkStatus = 'COMPLETED' ] )
              and not line_exists( workstatuss[ EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID  WorkStatus = 'STARTED' ]  )
              and not line_exists( workstatuss[ EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID  WorkStatus = 'SUSPENDED' ]  ).

          "check unconfirmed deviations
          check not line_exists( tour_confirmations_notes[ ServiceUUID = <service>-ServiceUUID ] ).

          create object service_processor.
          append value #( ServiceUUID = <service>-ServiceUUID ) to service_processor->service_complete.
          /plce/cl_bgprocessing_helper=>schedule(
                i_bgunit = service_processor
                i_associated_entities = value #( ( EntityType = /plce/if_pd_constants=>c_pd_entity_type-service EntityUUID = <service>-ServiceUUID ) )
              ).

        endloop.
      catch /plce/cx_baseexception into data(lex).
        failed-service = value #( for k in keys ( %key = corresponding #( k-%key ) %action-TryCompleteService = if_abap_behv=>mk-on ) ) .
        reported-service = value #( for k in keys ( %key = corresponding #( k-%key ) %action-TryCompleteService = if_abap_behv=>mk-on %msg = lex ) ) .
    endtry.
  endmethod.

  METHOD UnassignOnDelete.
    data: assigned_services type table for action import /PLCE/R_PDTour\\ServiceAssignment~UnAssign.

    select asgmt~ServiceUUID, asgmt~TourUUID from /PLCE/R_PDTourServiceAsgmt as asgmt
      inner join @keys as keys on asgmt~ServiceUUID = keys~ServiceUUID into corresponding fields of table @assigned_services.

    if assigned_services is not initial.
      modify entities of /PLCE/R_PDTour
        entity ServiceAssignment
          execute UnAssign from assigned_services
        failed data(srvc_failed)
        reported data(srvc_reported).

    /plce/cl_base_misc=>check_response(
      exporting
        is_response = srvc_reported
      changing
        ct_messages = reported-%other
    ).
    endif.
  ENDMETHOD.

  METHOD validateTasks.
    data: lt_service_tasks_sorted type standard table of /Plce/R_PDServicetask with non-unique sorted key sec_key components ServiceUUID.

    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceUUID ServiceId )
          with value #( for key in keys (
                          %tky = corresponding #( key-%tky ) ) )
    result data(lt_services)
    by \_ServiceTask fields ( ServiceUUID )
          with value #( for key in keys (
                          %tky = corresponding #( key-%tky ) ) )
    result data(lt_service_tasks).
    move-corresponding lt_service_tasks to lt_service_tasks_sorted.

    lt_services = filter #( lt_services except in lt_service_tasks_sorted using key sec_key where ServiceUUID = ServiceUUID ).
    loop at lt_services assigning field-symbol(<ls_service>).
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft ) to failed-service.
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft
                      %msg        = new /plce/cx_pd_exception(
                                      severity = if_abap_behv_message=>severity-error
                                      textid = /plce/cx_pd_exception=>CS_PDSrvcWithoutTask
                                      serviceid = <ls_service>-ServiceId
                                    )
                      %create = if_abap_behv=>mk-on
      ) to reported-service.
    endloop.
  ENDMETHOD.

  METHOD setInitialStatus.
    read entities of /PLCE/R_PDService in local mode
      entity Service
      fields ( ServiceStatus  ) with corresponding #( keys )
      result data(lt_services).

    delete lt_services where ServiceStatus is not initial.
    if lt_services is initial.
      return.
    endif.

    modify entities of /PLCE/R_PDService in local mode
      entity Service
        update fields ( ServiceStatus ) with value #( for ls_service in lt_services
             ( %tky          = ls_service-%tky
               ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-in_planning ) )
         create by  \_StatusHistory auto fill cid
           set fields with value #( for ls_service in lt_services
             ( %tky    = ls_service-%tky
               %target = value #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-in_planning ) ) ) )
      reported data(lt_ServiceReported).

    Reported = corresponding #( deep lt_ServiceReported ).
  ENDMETHOD.

  METHOD calc_changed_stamp.
    if keys is not initial.
      get time stamp field data(ts).

      select distinct TourUuid from /PLCE/P_PDTourAssignment
       for all entries in @keys
         where ServiceUuid = @keys-serviceuuid
       into table @data(lt_tour).

      if lt_tour is not initial.
        modify entities of /PLCE/R_PDTour entity Tour
          update fields ( ServiceAssignmentChangedAt )
            with value #( for ukey in lt_tour ( TourUUID = ukey-TourUUID ServiceAssignmentChangedAt = ts ) )
          reported data(ltour_reported).

        /plce/cl_base_misc=>check_response(
          exporting
            is_response = ltour_reported
          changing
            ct_messages = reported-%other
        ).
      endif.
    endif.
  ENDMETHOD.

endclass.

class lhc_pdservicetask definition inheriting from cl_abap_behavior_handler.

  private section.
    methods calculateplanningstatus for determine on modify
      importing keys for servicetask~calculateplanningstatus.
    methods calculateservicetaskid for determine on modify
      importing keys for servicetask~calculateservicetaskid.
    methods calculateduration for determine on modify
      importing keys for servicetask~calculateduration.
    methods cleargeoroute for determine on modify "save
      importing keys for servicetask~cleargeoroute.
    methods get_instance_features for instance features
      importing keys request requested_features for servicetask result result.
    methods deactivateondelete for determine on modify
      importing keys for servicetask~deactivateondelete.
    methods validateservicetasks for validate on save
      importing keys for servicetask~validateservicetasks.
    types:
        tservicetaskreadresult type structure for read result /plce/r_pdservice\\servicetask.
endclass.

class lhc_pdservicetask implementation.

  method calculateplanningstatus.
    data: create_history type table for create /PLCE/R_PDService\_StatusHistory.

    read entities of /plce/r_pdservice in local mode
      entity servicetask fields ( touruuid planningstatus )
      with corresponding #( keys )
      result data(servicetasks).

    "remove up-to-date items
    delete servicetasks
      where ( touruuid is initial and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-unplanned )
      or ( touruuid is not initial and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-planned  ).

    "update service tasks
    loop at servicetasks assigning field-symbol(<servicetask>).
      <servicetask>-planningstatus = cond #( when <servicetask>-touruuid is initial then /plce/if_pd_constants=>c_pd_planning_status-unplanned else /plce/if_pd_constants=>c_pd_planning_status-planned ).
    endloop.

    modify entities of  /plce/r_pdservice in local mode
      entity servicetask
      update fields ( planningstatus ) with corresponding #( servicetasks ).

    "read all tasks for affected services
    read entities of /plce/r_pdservice in local mode
      entity service
        by \_servicetask fields ( serviceuuid planningstatus )
          with corresponding #( servicetasks )
          result data(all_servicestasks)
        fields ( planningstatus ) with corresponding #( servicetasks )
          result data(services).

    "check all affected services
    loop at services assigning field-symbol(<service>).
      types _t type table of /plce/pdservice_task_uuid with default key.
      data(iplanned) = lines( value _t( for line in all_servicestasks where ( serviceuuid = <service>-serviceuuid and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-planned ) ( line-servicetaskuuid ) ) ).
      data(iunplanned) = lines( value _t( for line in all_servicestasks where ( serviceuuid = <service>-serviceuuid and planningstatus = /plce/if_pd_constants=>c_pd_planning_status-unplanned ) ( line-servicetaskuuid ) ) ).
      data target_status type /plce/pdplanning_status.
      if  iplanned > 0 and iunplanned > 0.
        target_status = /plce/if_pd_constants=>c_pd_planning_status-partially_planned.
      elseif iplanned = 0.
        target_status = /plce/if_pd_constants=>c_pd_planning_status-unplanned.
      else.
        target_status = /plce/if_pd_constants=>c_pd_planning_status-planned.
      endif.

      if <service>-planningstatus = target_status.
        delete services.
      else.
        <service>-planningstatus = target_status.
        append value #( %tky = <service>-%tky
                        %target = value #( ( PlanningStatus = target_status ) ) ) to create_history.
      endif.
    endloop.

    "update affected/changed services
    modify entities of  /plce/r_pdservice in local mode
      entity service
        update fields ( planningstatus ) with corresponding #( services )
    "create status history
        create by  \_StatusHistory auto fill cid set fields with create_history
      mapped data(lservice_mapped)
      reported data(lservice_reported).

    read entities of /PLCE/R_PDService in local mode
      entity StatusHistory
      fields ( PlanningStatus ) with corresponding #( lservice_mapped-statushistory )
      result data(lservice_histories).

    reported = corresponding #( deep lservice_reported ).
  endmethod.

  method calculateservicetaskid.
    read entities of /plce/r_pdservice in local mode
      entity servicetask
        all fields with corresponding #( keys )
      result data(lt_service_tasks).

    delete lt_service_tasks where servicetaskid is not initial.
    if lines( lt_service_tasks ) > 0.
      try.
          data(lnumber) = /plce/cl_base_misc=>get_next_numbers(
            iv_nr_range_nr = '01'
            iv_object      = '/PLCE/PDST'
            iv_count       = lines( lt_service_tasks )
          ).
          if lnumber is not initial.
            modify entities of /plce/r_pdservice in local mode
              entity servicetask
                update
                  from value #( for <key> in keys index into i
                  let lid = conv /plce/pdservice_task_id( lnumber + i ) in
                  (
                    %tky = <key>-%tky
                    servicetaskid = |{ lid alpha = in }|
                    %control-servicetaskid = if_abap_behv=>mk-on ) )
              reported data(update_reported).

            reported = corresponding #( deep update_reported ).
          else.
            raise exception type /plce/cx_baseexception
              exporting
                textid      = /plce/cx_baseexception=>no_number_received
                attribute_1 = '/PLCE/PDST'
                attribute_2 = '01'.
          endif.
        catch /plce/cx_baseexception into data(lex).
          insert lex into table reported-%other.
      endtry.
    endif.
  endmethod.


  method calculateduration.
    read entities of /PLCE/R_PDService in local mode
      entity ServiceTask
        fields ( TaskType SequenceNumber )
          with corresponding #( keys )
          result data(tasks)
        by \_service fields ( ServiceType Profile )
          with corresponding #( keys )
            result data(services)
            .

    " select involved templates
    select distinct template~servicetype, template~tasktype, template~sqncnr, template~profile, template~duration, template~durationunit
    from /PLCE/R_PDServiceTaskTmpl as template
      inner join @services as services
        on template~profile = services~Profile and template~ServiceType = services~ServiceType
    into table @data(tasktemlates).

    " select all FuncLocCats
    select * from /PLCE/R_MDFuncLocMDCat
    into table @data(funcloccats). "#EC CI_ALL_FIELDS_NEEDED "#EC CI_NOWHERE

    " adjust duration/unit
    loop at tasks assigning field-symbol(<task>).
      case <task>-functionallocationcategory.
        when /plce/if_md_constants=>c_funcloc_mdcategories-customerlocation.
          data(template) = value #( tasktemlates[
              Profile = services[ key entity ServiceUUID = <task>-ServiceUUID ]-Profile
              ServiceType = services[ key entity ServiceUUID = <task>-ServiceUUID ]-ServiceType
              TaskType = <task>-TaskType SqncNr = <task>-SequenceNumber ] optional ).
          <task>-duration = template-duration.
          <task>-durationunit = template-durationunit.

        when /plce/if_md_constants=>c_funcloc_mdcategories-materialdepot or /plce/if_md_constants=>c_funcloc_mdcategories-transportpackagingdepot.
          if <task>-duration is initial or <task>-durationunit is initial.
            data(flc) = value #( funcloccats[ funclocmdcategory = <task>-functionallocationcategory ] optional ).
            if sy-subrc is initial.
              <task>-duration = flc-stayduration.
              <task>-durationunit = flc-staydurationunit.
            endif.
          endif.
        when others.
      endcase.
    endloop.

    "update tasks
    modify entities of /PLCE/R_PDService in local mode
      entity ServiceTask
        update fields ( Duration DurationUnit )
          with value #( for t in tasks ( %tky = t-%tky Duration = t-Duration DurationUnit = t-DurationUnit ) )
      reported data(lservice_reported).

    reported = corresponding #( deep lservice_reported ).
  endmethod.


  method ClearGeoRoute.
    read entities of /PLCE/R_PDService in local mode
      entity ServiceTask fields ( ServiceTaskUUID TourUUID ) with corresponding #( keys )
      result data(lt_servicetask).

    read entities of /PLCE/R_PDTour
      entity Tour fields ( TourUUID ) with corresponding #( lt_servicetask )
      result data(lt_tour).

    if lt_tour is not initial.
      modify entities of /PLCE/R_PDTour
        entity Tour execute DeleteGeoRoute from corresponding #( lt_tour )
        reported data(ltour_reported)
        failed data(ltour_failed).

      /plce/cl_base_misc=>check_response(
        exporting
          is_response = ltour_reported
        changing
          ct_messages = reported-%other
      ).
    endif.
  endmethod.

  method get_instance_features.
    read entities of /PLCE/R_PDService in local mode
      entity ServiceTask
        fields ( ServiceUUID ServiceStatus ) with corresponding #( keys )
        result data(service_tasks).

     result =
        value #( FOR st IN service_tasks
                       ( %tky                = st-%tky
                         %delete             = COND #( WHEN st-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-confirmed
                                                         OR st-ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-completed
                                                       THEN if_abap_behv=>fc-o-disabled
                                                       ELSE if_abap_behv=>fc-o-enabled ) )
             ).
  endmethod.

  method DeactivateOnDelete.
    data: assigned_tasks type table for action import /PLCE/R_PDTour\\TaskAssignment~Deactivate.

    select st~ServiceTaskUUID, st~ServiceUUID, st~TourUUID, 'S' as TaskScope from /PLCE/R_PDServiceTask as st
      inner join @keys as keys on st~ServiceTaskUUID = keys~ServiceTaskUUID where TourUUID is not initial into corresponding fields of table @assigned_tasks.

    if assigned_tasks is not initial.
      modify entities of /PLCE/R_PDTour
        entity TaskAssignment
          execute Deactivate from assigned_tasks
        failed data(st_failed)
        reported data(st_reported).

    /plce/cl_base_misc=>check_response(
      exporting
        is_response = st_reported
      changing
        ct_messages = reported-%other
    ).
    endif.
  endmethod.

  METHOD validateServiceTasks.
    data: lt_service_tasks_sorted type standard table of /Plce/R_PDServicetask with non-unique sorted key sec_key components ServiceUUID.

    " read ServiceUUID before commit
    select distinct st~ServiceUUID from /PLCE/R_PDServiceTask as st
      inner join @keys as keys on st~ServiceTaskUUID = keys~ServiceTaskUUID
      into table @data(lt_service_keys).

    read entities of /PLCE/R_PDService in local mode
      entity Service
        fields ( ServiceUUID ServiceId )
          with value #( for key in lt_service_keys (
                          ServiceUUID = key-ServiceUUID ) )
    result data(lt_services)
    by \_ServiceTask fields ( ServiceUUID )
          with value #( for key in lt_service_keys (
                          ServiceUUID = key-ServiceUUID ) )
    result data(lt_service_tasks).
    move-corresponding lt_service_tasks to lt_service_tasks_sorted.

    lt_services = filter #( lt_services except in lt_service_tasks_sorted using key sec_key where ServiceUUID = ServiceUUID ).
    loop at lt_services assigning field-symbol(<ls_service>).
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft ) to failed-servicetask.
      append value #( %key      = <ls_service>-%key
                      %is_draft = <ls_service>-%is_draft
                      %msg        = new /plce/cx_pd_exception(
                                      severity = if_abap_behv_message=>severity-error
                                      textid = /plce/cx_pd_exception=>CS_PDSrvcWithoutTask
                                      serviceid = <ls_service>-ServiceId
                                    )
                      %delete = if_abap_behv=>mk-on
      ) to reported-servicetask.
    endloop.
  ENDMETHOD.

endclass.

class lhc_attachments definition inheriting from cl_abap_behavior_handler.

  private section.

    methods validateattachment for validate on save
      importing keys for attachments~validateattachment.

endclass.

class lhc_attachments implementation.
  method validateattachment.
    read entities of /plce/r_pdservice in local mode
      entity attachments
        fields ( attachment )
        with corresponding #( keys )
        result data(lt_service_attachments).

    loop at lt_service_attachments assigning field-symbol(<ls_service_attachment>) where attachment is initial.
      append value #( %key      = corresponding #( <ls_service_attachment>-%key )
                      %is_draft = <ls_service_attachment>-%is_draft ) to failed-service.
      append value #( %key      = corresponding #( <ls_service_attachment>-%key )
                      %is_draft = <ls_service_attachment>-%is_draft
                      %msg        = new /plce/cx_pd_exception(
                                        severity = if_abap_behv_message=>severity-error
                                        textid = /plce/cx_pd_exception=>cs_service_attachment_empty
                                    )
                      %create = if_abap_behv=>mk-on
      ) to reported-service.
    endloop.
  endmethod.
endclass.

projection;
strict ( 2 );
extensible;

define behavior for /PLCE/C_PDMNLServiceWR alias Service
extensible
{

  use action assignTour;
  use action createTour;
//  use action planAutomatically;

  use association _ServiceTask { create; }
}

define behavior for /PLCE/C_PDMNLServiceTaskWR alias ServiceTask
{
  use association _Service { }
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
