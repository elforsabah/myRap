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


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'PD Service Extension Custom'
@AbapCatalog.extensibility: { extensible: true } 
@ObjectModel.usageType:{
  serviceQuality: #A,
  sizeCategory: #L,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDServiceExtCustom
  as select from /plce/tpdsrvcst
  association to parent /PLCE/R_PDService as _Service on $projection.ServiceUUID = _Service.ServiceUUID
{
  key service_uuid as ServiceUUID,
      _Service
}

extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association to ewa_order_object as _ewa_order_object on $projection.ZZPOBJNR = _ewa_order_object.pobjnr
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _ZZOrderObject,
  _ewa_order_object
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

We have this Main CDS view 

/PLCE/R_PDService (not extendable )

which has a composition of /PLCE/R_PDServiceExtWR  (not extendatble)

and 

/PLCE/R_PDServiceExtCustom  (extendable)

so to add a new field to this main cds you have to use the /PLCE/R_PDServiceExtCustom which I have use like so. 

extend view entity /PLCE/R_PDServiceExtCustom with
association [1] to I_EWA_WasteDisposalOrderItem as _ZZOrderObject on _ZZOrderObject.EWAWasteDsplOrdItmObjectNumber = $projection.ZZPOBJNR
association to ewa_order_object as _ewa_order_object on $projection.ZZPOBJNR = _ewa_order_object.pobjnr
{
  _Service.ReferenceInternalId as ZZPOBJNR,
  _ZZOrderObject,
  _ewa_order_object
}


The ewa_order_object has a field calle zzwdio which i want to extend the /PLCE/R_PDServiceExtWR with . that is this field should be available on this CDS view 

The want to use this field to then calculate the criticallity of the entire row and set it to red if the value of this field is = 4 

The critically shopuld be in the metadata extension the extends /PLCE/R_PDServiceExtWR. 


Provide a step by step procedure on how this can be done. 

Please be clear consise and detailed about everything
