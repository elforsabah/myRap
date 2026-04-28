<img width="1770" height="838" alt="image" src="https://github.com/user-attachments/assets/794c00d4-bb36-40dc-867a-8b22c3b1781f" />


<img width="1719" height="890" alt="image" src="https://github.com/user-attachments/assets/cf3fefb9-1af3-464b-8038-28add2f7c334" />


****<img width="1492" height="284" alt="image" src="https://github.com/user-attachments/assets/530e81f9-2f0a-458f-9d43-f72d17617a16" />


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Manual Service'
@Search.searchable: true
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #XL,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDMNLService
  provider contract transactional_query
  as projection on /PLCE/R_PDService
{
          @UI.hidden: true
  key     ServiceUUID,
          ServiceId,
          @UI.hidden: true
          Profile,

          @ObjectModel.text.element: ['ServiceTypeDescription']
          @Search.defaultSearchElement: true
          @UI.textArrangement: #TEXT_ONLY
          ServiceType,

          @UI.hidden: true
          _ServiceType._ServiceTypeText.ServiceTypeDescription as ServiceTypeDescription : localized,

          @Search.defaultSearchElement: true
          ReferenceId,
          ServicePriority,

          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDPLANNINGSTATUS_VH', element : 'StatusDescription'} }]
          _PlanningStatusText.Description                      as PlanningStatusText     : localized,

          @ObjectModel.text.element: ['PlanningStatusText']
          @Consumption.valueHelpDefinition: [{ entity : { name: '/PLCE/C_PDPLANNINGSTATUS_VH', element : 'PlanningStatus'} }]
          @Consumption.filter: { defaultValue: '', multipleSelections: true, selectionType: #SINGLE }
          @UI.textArrangement: #TEXT_ONLY
          PlanningStatus,

          @UI.hidden: true
          _ServiceLookup.PlanningStatusCriticality             as PlanningStatusCriticality,

          @Consumption.filter: { mandatory: true, hidden: false, selectionType: #INTERVAL }
          RequestedDate,
          EarliestDate,
          LatestDate,

          @Search.defaultSearchElement: true
          CustomerInfo                                         as CustomerInfo,
          @UI.multiLineText: true
          AdditionalText,

          @EndUserText.label: 'Functional Location'
          FunctionalLocation,
          _FunctionalLocation.FullAddress                      as FullAddress,

          @EndUserText.label: 'Service Window Description'
          @ObjectModel.text.element: ['ServiceWindowText']
          @UI.textArrangement: #TEXT_ONLY
          ServiceWindow,
          @UI.hidden: true
          _ServiceWindowText[1:Language = $session.system_language].Description          as ServiceWindowText,

          ServiceWindowStartTime,
          ServiceWindowEndTime,

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

          //Extension WR
          //          _ExtWaste.Material as Material_WR,
          //          _ExtWaste.MaterialWeight as MaterialWeight,
          //          _ExtWaste.MaterialWeightUnit as MaterialWeightUnit,
          //          _ExtWaste.PlantLocation as PlantLocation,
          //          _ExtWaste.ContainerSourceLocation as ContainerSourceLocation,
          //          _ExtWaste.ContainerFinalLocation as ContainerFinalLocation,
          //          _ExtWaste.ContainerTypeAtLocation as ContainerTypeAtLocation,
          //          _ExtWaste.ContainerAtLocationCount as ContainerAtLocationCount,
          //         _ExtWaste.ContainerAtLocationTidnr as ContainerAtLocationTidnr,
          //         _ExtWaste.ContainerTypeNew as ContainerTypeNew,
          //         _ExtWaste.ContainerNewCount as ContainerNewCount,
          //         _ExtWaste.ContainerNewTidnr as ContainerNewTidnr,
          //

           /* Associations */
          _ServiceTask        : redirected to composition child /PLCE/C_PDMNLServiceTask,
          _Attachments        : redirected to composition child /PLCE/C_PDMNLServiceAttachment,
          _FunctionalLocation : redirected to /PLCE/C_PDMNLFuncLoc,
          _WorkAreaServices,
          _TourAssignments,
          _PlanningStatusText
}



