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
         //         /PLCE/R_PDTour._Profiles as _Profile
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

