@EndUserText.label: 'Manual Tour'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Search.searchable: true
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLTour
  provider contract transactional_query
  as projection on /PLCE/R_PDTour
{
  key    TourUUID,
         @Search.defaultSearchElement: true
         TourId,

         @EndUserText.label: 'Template'
         @ObjectModel.text.element: ['TourTemplateName']
         @Search.defaultSearchElement: true
         TourTemplate,
         @EndUserText.label: 'Template Description'
         _TourTemplate._Text.TourTemplateName                                            as TourTemplateName : localized,

         @ObjectModel.text.element: ['TourStatusText']
         TourStatus,
         @Semantics.text: true
         @UI.hidden: true
         _PDTourStatusText.Description                                                   as TourStatusText   : localized,
         @UI.hidden: true
         _TourLookup.TourStatusColorValue                                                as TourStatusColorValue,

         @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
         StartDate                                                                       as TourStartDate,
         EndDate                                                                         as TourEndDate,
         ScheduledDateTimeStart                                                          as ScheduledDateTimeStart,

         @Semantics.quantity.unitOfMeasure: 'CalculatedTourDurationUnit'
         CalculatedTourDuration                                                          as TourDuration,
         @UI.hidden: true
         cast( CalculatedTourDurationUnit as meins preserving type ) as CalculatedTourDurationUnit, //show localized key/allow fiori app value check
         CalculatedTourCapacity                                                          as TourCapacity,
         _TourLookup.TourCapacityColorValue                                              as TourCapacityColorValue,

         MainResourceId                                                                  as MainResource,

         EarliestStartTime,
         //LatestStartTime,

         @Semantics.quantity.unitOfMeasure: 'MaximumTourDurationUnit'
         MaximumTourDuration,
         @UI.hidden: true
         cast( MaximumTourDurationUnit as meins preserving type ) as MaximumTourDurationUnit, //show localized key/allow fiori app value check

         @EndUserText.label: 'Tour Start Functional Location'
         @ObjectModel.text.element: ['StartFLocFullAddress']
         StartFunctionalLocation,
         _PDStartFunctionalLocation._FuctionalLocationText.FunctionalLocationDescription as StartFLocName    : localized,
         @EndUserText.label: 'Start Address'
         @Semantics.text: true
         _PDStartFunctionalLocation.FullAddress                                          as StartFLocFullAddress,
         @UI.hidden: true
         _PDStartFunctionalLocation.Latitude                                             as StartLatitude,
         @UI.hidden: true
         _PDStartFunctionalLocation.Longitude                                            as StartLongitude,
         @EndUserText.label: 'Tour End Functional Location'
         @ObjectModel.text.element: ['EndFLocFullAddress']
         EndFunctionalLocation,
         _PDEndFunctionalLocation._FuctionalLocationText.FunctionalLocationDescription   as EndFLocName      : localized,
         @EndUserText.label: 'End Address'
         @Semantics.text: true
         _PDEndFunctionalLocation.FullAddress                                            as EndFLocFullAddress,
         @UI.hidden: true
         _PDEndFunctionalLocation.Latitude                                               as EndLatitude,
         @UI.hidden: true
         _PDEndFunctionalLocation.Longitude                                              as EndLongitude,

         ExternalTourID,

         @UI.hidden: true
         CreatedForSimulation,

//         PDProfile,

         @EndUserText.label: 'Work Status Description'
         @ObjectModel.text.element: ['WorkStatusText']
         @UI.textArrangement: #TEXT_ONLY
         _TourLookup.WorkStatus,
         @UI.hidden: true
         _TourLookup._WorkStatusText.Description                                         as WorkStatusText   : localized,
         _TourLookup._WorkStatusText.IconURL                                             as WorkStatusIcon   : localized,
         @EndUserText.label: 'Driving Time'
         @Semantics.quantity.unitOfMeasure: 'DrivingTimeUnit'
         _TourLookup.DrivingTime                                                         as DrivingTime,
         @UI.hidden: true
         _TourLookup.DrivingTimeUnit                                                     as DrivingTimeUnit,


         @UI.hidden: true
         CreatedBy,
         @UI.hidden: true
         CreatedAt,
         @UI.hidden: true
         LastChangedBy,
         @UI.hidden: true
         LastChangedAt,
         @UI.hidden: true
         LocalLastChangedAt,

         /*Associations*/
         _ServiceAssignments  : redirected to composition child /PLCE/C_PDMNLTourServiceAsgmt,
         _TaskAssignments     : redirected to composition child /PLCE/C_PDMNLTourTaskAsgmt,
         _Resources           : redirected to composition child /PLCE/C_PDMNLTourResource,
         _Attachments         : redirected to composition child /PLCE/C_PDMNLTourAttachment,
         _GeoRoutes           : redirected to composition child /PLCE/C_PDMNLTourGeoRoute,
         _WorkAreaTours         


}
