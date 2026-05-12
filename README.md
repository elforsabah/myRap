<img width="1334" height="217" alt="image" src="https://github.com/user-attachments/assets/08bac14e-8b56-4628-a15c-930895426f8a" />


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Manual Service Modify'
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLService_M
  provider contract transactional_query
  as projection on /PLCE/R_PDService
  association [1]    to /PLCE/C_PDServiceC              as _ServiceCore          on _ServiceCore.ServiceUUID = $projection.ServiceUUID
{
  key     ServiceUUID,
          ServiceId,

          Profile,

          @ObjectModel.text.element: ['ServiceTypeDescription']
          ServiceType,


          @UI.hidden: true
          _ServiceType._ServiceTypeText.ServiceTypeDescription as ServiceTypeDescription : localized,

          ReferenceId,
          ServicePriority,

          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDPLANNINGSTATUS_VH', element : 'StatusDescription'} }]
          _PlanningStatusText.Description                      as PlanningStatusText     : localized,

          @ObjectModel.text.element: ['PlanningStatusText']
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDPLANNINGSTATUS_VH', element : 'PlanningStatus'} }]
          @Consumption.filter: { defaultValue: '', multipleSelections: true, selectionType: #SINGLE }
          PlanningStatus,

          @UI.hidden: true
          _ServiceLookup.PlanningStatusCriticality             as PlanningStatusCriticality,

          @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
          RequestedDate,
          EarliestDate,
          LatestDate,

          CustomerInfo                                         as CustomerInfo,
          @UI.multiLineText: true
          AdditionalText,

          @EndUserText.label: 'Functional Location'
          @ObjectModel.text.element: [ 'FunctionalLocationDescription' ]
          FunctionalLocation,
          _FunctionalLocation.FullAddress                      as FullAddress,
          _FunctionalLocation._FuctionalLocationText.FunctionalLocationDescription : localized,


          @Semantics.unitOfMeasure: true
          cast( _Statistic.TotalDurationUnit as meins preserving type ) as TotalDurationUnit, //show localized key/allow fiori app value check
          @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
          _Statistic.TotalDuration,

          @EndUserText.label: 'Work Status Description'
          @ObjectModel.text.element: ['WorkStatusText']
          @UI.textArrangement: #TEXT_ONLY
          _ServiceLookup.WorkStatus,
          @UI.hidden: true
          _ServiceLookup._WorkStatusText.Description           as WorkStatusText         : localized,
          _ServiceLookup._WorkStatusText.IconURL               as WorkStatusIcon         : localized,

          @ObjectModel.text.element: ['ServiceWindowDescription']
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDServiceWindow_VH', element : 'ServiceWindow'} }]
          @UI.textArrangement: #TEXT_ONLY
          ServiceWindow,
          _ServiceWindowText.Description as ServiceWindowDescription : localized,
          ServiceWindowStartTime,
          ServiceWindowEndTime,
          @UI.hidden: true
          _ServiceLookup.HideWrData,


          CreatedAt,
          CreatedBy,
          LastChangedAt,
          LastChangedBy,
          LocalLastChangedAt,

          /* Associations */
          _ServiceCore,
          _Attachments        : redirected to composition child /PLCE/C_PDMNLServiceAttchmnt_M,
          _WorkAreaServices,
          _PlanningStatusText,
          _TourAssignments,
          _Notes              : redirected to composition child /PLCE/C_PDMNLServiceNote_M,
          _ExtWaste           : redirected to composition child /PLCE/C_PDMNLServiceWR_M

}

