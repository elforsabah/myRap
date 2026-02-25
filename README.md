@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour Assignments'
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/P_PDTourAssignment
  as select from /PLCE/R_PDTourServiceAsgmt
{
  key TourUUID,
  key ServiceUUID,

      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourWA_VH', element: 'TourId' } }]
      _Tour.TourId,
      _Service.ServiceId,

      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDCustomerInfoWA_VH', element: 'CustomerInfo' } }]
      _Service.CustomerInfo,
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocServiceWA_VH', element: 'FullAddress' } }]
      @EndUserText.label: 'Service Address'
      _Service._FunctionalLocation.FullAddress,

      Removed,
      TourSequence,

      /*Assocaitions*/
      _Tour,
      _Service
}
where
  Removed is initial



@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Tour - Service Assignment'
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDTourServiceAsgmt
  as select from /plce/tpdtassrvc
  association        to parent /PLCE/R_PDTour        as _Tour                      on  _Tour.TourUUID = $projection.TourUUID
  association [1]    to /PLCE/R_PDService            as _Service                   on  _Service.ServiceUUID = $projection.ServiceUUID
  association [1..1] to /PLCE/R_PDTourServiceAsgmt_L as _Lookup                    on  _Lookup.TourUUID    = $projection.TourUUID
                                                                                   and _Lookup.ServiceUUID = $projection.ServiceUUID
  association [0..*] to /PLCE/R_PDTourTaskAssignment as _ServiceTasksAssignments   on  _ServiceTasksAssignments.TourUUID    = $projection.TourUUID
                                                                                   and _ServiceTasksAssignments.TaskScope   = 'S'
                                                                                   and _ServiceTasksAssignments.ServiceUUID = $projection.ServiceUUID
  //and _ServiceTasksAssignments.Removed is initial
  /*+[hideWarning] { "IDS" : [ "CARDINALITY_CHECK" ]  } */
  association [0..1] to /PLCE/R_PDTourTaskAssignment as _LeadServiceTaskAssignment on  _LeadServiceTaskAssignment.TourUUID        = $projection.TourUUID
                                                                                   and _LeadServiceTaskAssignment.TaskScope       = 'S'
                                                                                   and _LeadServiceTaskAssignment.ServiceUUID     = $projection.ServiceUUID
                                                                                   and _LeadServiceTaskAssignment.ServiceTaskUUID = $projection.LeadServiceTaskUUID
  //and _LeadServiceTaskAssignment.TourTaskUUID = $projection.emptyUUID



{
  key tour_uuid                               as TourUUID,
  key service_uuid                            as ServiceUUID,
      lead_service_task_uuid                  as LeadServiceTaskUUID,
      _LeadServiceTaskAssignment.TourSequence as TourSequence,
      @EndUserText.label: 'Removed'
      removed                                 as Removed,
      _Service.ServiceStatus                  as ServiceStatus,
      @Semantics.user.createdBy: true
      @EndUserText.label: 'Created By'
      created_by                              as CreatedBy,
      @Semantics.systemDateTime.createdAt: true
      @EndUserText.label: 'Created At'
      created_at                              as CreatedAt,
      @Semantics.user.lastChangedBy: true
      @EndUserText.label: 'Last Changed By'
      last_changed_by                         as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      @EndUserText.label: 'Last Changed At'
      last_changed_at                         as LastChangedAt,
      @Semantics.systemDateTime.localInstanceLastChangedAt: true
      @EndUserText.label: 'Local Last Changed At'
      local_last_changed_at                   as LocalLastChangedAt,
      /* Associations */
      _Tour,
      _Lookup,
      _Service,
      _ServiceTasksAssignments,
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


extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend  behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
action ( precheck, features : global ) adjusttime parameter ZAE_D_adjusttime_AB result [0..*] $self;

 determination Setfachbereich on modify { create; update; }

 // Tell the UI that running this action affects the parent Tour (Tour list will be refreshed)
  side effects {
    action adjusttime affects entity _TourAssignments;
  }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
    zz_vehicleinfo = zz_vehicleinfo;
    zz_discrepancy = zz_discrepancy;
    zz_reactiontime =  zz_reactiontime;
    zz_order_date = zz_order_date;
    zz_timeadjustment = zz_timeadjustment;
 }

field ( readonly : update ) zz_tech_fachbe;
field ( readonly : update ) zz_vehicleinfo;
field ( readonly : update ) zz_discrepancy;
field ( readonly : update ) zz_reactiontime;
field ( readonly : update ) zz_order_date;
field ( readonly : update ) zz_timeadjustment;
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
@EndUserText.label: 'Tour WR'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Search.searchable: true
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLTourWR
  provider contract transactional_query
  as projection on /PLCE/R_PDTour
  association [1] to /PLCE/C_PDMNLTour as _TourCore on _TourCore.TourUUID = $projection.TourUUID
{
  key    TourUUID,
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
         _TourTemplate._Text.TourTemplateName                                            as TourTemplateName : localized,

         @UI.hidden: true
         _TourTemplate.ColorTour                                                         as ColorTour,

         @EndUserText.label: 'Tour Status'
         @ObjectModel.text.element: ['TourStatusText']
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDTourStatus_VH', element: 'TourStatus' } }]
         @Consumption.filter: { multipleSelections: true, selectionType: #SINGLE }
         TourStatus,
         @Semantics.text: true
         @UI.hidden: true
         _PDTourStatusText.Description                                                   as TourStatusText   : localized,
         @UI.hidden: true
         _TourLookup.TourStatusColorValue                                                as TourStatusColorValue,
         @Consumption.filter.hidden: true
         _PDTourStatusText.IconURL                                                       as TourStatusIcon   : localized,

         @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
         StartDate                                                                       as TourStartDate,
         @Consumption.filter.selectionType: #INTERVAL
         EndDate                                                                         as TourEndDate,
         @Consumption.filter.selectionType: #INTERVAL
         ScheduledDateTimeStart                                                          as ScheduledDateTimeStart,

         @Semantics.quantity.unitOfMeasure: 'CalculatedTourDurationUnit'
         @Consumption.filter.hidden: true
         CalculatedTourDuration                                                          as TourDuration,
         @UI.hidden: true
         cast( CalculatedTourDurationUnit as meins preserving type )                     as CalculatedTourDurationUnit, //show localized key/allow fiori app value check
         @Consumption.filter.hidden: true
         CalculatedTourCapacity                                                          as TourCapacity,
//         @Consumption.filter.hidden: true
         @UI.hidden: true
         _TourLookup.TourCapacityColorValue                                              as TourCapacityColorValue,

         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDResource_VH', element: 'ResourceId'} }]
         MainResourceId                                                                  as MainResource,

         @Consumption.filter.selectionType: #SINGLE
         EarliestStartTime,
         //LatestStartTime,

         @Semantics.quantity.unitOfMeasure: 'MaximumTourDurationUnit'
         @Consumption.filter.hidden: true
         MaximumTourDuration,
         @UI.hidden: true
         cast( MaximumTourDurationUnit as meins preserving type )                        as MaximumTourDurationUnit, //show localized key/allow fiori app value check

         @EndUserText.label: 'Start Address'
         @Semantics.text: true
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDNoCustomerLocWA_VH', element: 'FullAddress' },
                                              label            : 'Start Functional Location'
                                              }]
         _PDStartFunctionalLocation.FullAddress                                          as StartFLocFullAddress,
         @UI.hidden: true
         _PDStartFunctionalLocation._FuctionalLocationText.FunctionalLocationDescription as StartFLocName    : localized,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDStartFunctionalLocation.Latitude                                             as StartLatitude,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDStartFunctionalLocation.Longitude                                            as StartLongitude,

         @EndUserText.label: 'End Address'
         @Semantics.text: true
         @ObjectModel.text.element: [ 'EndFLocFullAddress' ]
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDNoCustomerLocWA_VH', element: 'FullAddress' },
                                              label            : 'End Functional Location'
                                              }]
         _PDEndFunctionalLocation.FullAddress                                            as EndFLocFullAddress,
         @UI.hidden: true
         _PDEndFunctionalLocation._FuctionalLocationText.FunctionalLocationDescription   as EndFLocName      : localized,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDEndFunctionalLocation.Latitude                                               as EndLatitude,
         @UI.hidden: true //@Consumption.filter.hidden: true
         _PDEndFunctionalLocation.Longitude                                              as EndLongitude,

          //map
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapTitle     : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapPopup     : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapColor     : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapHighlight : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_S_CALC'
  virtual StartMapSymbol    : abap.string,

          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapTitle       : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapPopup       : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapColor       : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapHighlight   : abap.string,
          @ObjectModel.virtualElementCalculatedBy: 'ABAP:/PLCE/CL_PDMNL_TOUR_LOC_E_CALC'
  virtual EndMapSymbol      : abap.string,

         ExternalTourID,

         @UI.hidden: true
         CreatedForSimulation,

         //         PDProfile,
         @EndUserText.label: 'Route available'
         _TourLookup.IsGeoRouteAvailable,
         @EndUserText.label: 'Work Status'
         @UI.textArrangement: #TEXT_ONLY
         @ObjectModel.text.element: ['WorkStatusText']
         @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDWorkStatus_VH', element: 'Value'} }]
         _TourLookup.WorkStatus,
         @UI.hidden: true
         _TourLookup._WorkStatusText.Description                                         as WorkStatusText   : localized,
         @Consumption.filter.hidden: true
         _TourLookup._WorkStatusText.IconURL                                             as WorkStatusIcon   : localized,

         @EndUserText.label: 'Driving Time'
         @Consumption.filter.hidden: true
         _TourLookup.DrivingTime                                                         as DrivingTime,
         @UI.hidden: true
         _TourLookup.DrivingTimeUnit                                                     as DrivingTimeUnit,

         @UI.hidden: true
         _TourLookup._TourCriticality.TourCriticality                                    as TourCriticality,
         @EndUserText.label: 'Deviations on Tour'
         _TourLookup._TourCriticality.DeviationsOnTour                                   as DeviationsOnTour,

         @UI.hidden: true
         _TourLookup._TourLookupWR.WeightUnit                                            as WeightUnit,
         @Consumption.filter.hidden: true
         _TourLookup._TourLookupWR.MaterialWeightInTOSum                                 as MaterialWeightInTOSum,
         @UI.hidden: true
         @Semantics.quantity.unitOfMeasure: 'WeightUnit'
         _TourLookup._TourLookupWR.VehicleCapacityInTOSum                                as VehicleCapacityInTOSum,
         @Consumption.filter.hidden: true
         @UI.hidden: true
         _TourLookup._TourLookupWR.CriticalTonnage                                       as CriticalityTonnage,

         @UI.hidden: true
         _TourLookup._TourLookupWR.CountServices                                         as CountServices,
         @UI.hidden: true
         _TourLookup._TourLookupWR.CountServicesCompleted                                as CountServicesCompleted,

         /*Associations*/
         _TourCore,
         _ServiceAssignments  : redirected to composition child /PLCE/C_PDMNLTourServiceAsgWR,
         _TaskAssignments     : redirected to composition child /PLCE/C_PDMNLTourTaskAsgmtWR,
         _Resources           : redirected to /PLCE/C_PDMNLTourResource,
         _Attachments         : redirected to /PLCE/C_PDMNLTourAttachment,
         _FunctionalLocations : redirected to /PLCE/C_PDMNLTourFuncLoc,
         _GeoRoutes           : redirected to /PLCE/C_PDMNLTourGeoRoute,
         _WorkAreaTours
         //         /PLCE/R_PDTour._Profiles as _Profiles

}
