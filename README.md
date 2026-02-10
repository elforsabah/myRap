class ZCL_WR_TOUR_EXTEND_CALC definition
  public
  create public .

public section.

  interfaces IF_SADL_EXIT .
  interfaces IF_SADL_EXIT_CALC_ELEMENT_READ .
protected section.
private section.
ENDCLASS.



CLASS ZCL_WR_TOUR_EXTEND_CALC IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_TOUR_EXTEND_CALC->IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_ORIGINAL_DATA               TYPE        STANDARD TABLE
* | [--->] IT_REQUESTED_CALC_ELEMENTS     TYPE        TT_ELEMENTS
* | [<-->] CT_CALCULATED_DATA             TYPE        STANDARD TABLE
* | [!CX!] CX_SADL_EXIT
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
  FIELD-SYMBOLS: <ls_original>            TYPE any,
                 <ls_calculated>          TYPE any,
                 <lv_refid>               TYPE any,
                 <lv_service_criticality> TYPE int1,
                 <lv_reactiontime>        TYPE zwr_d_ewmd_remaining_duration.

  CLEAR ct_calculated_data.
*  LOOP AT it_original_data ASSIGNING <ls_original>.
*    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
*    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.
*
*    ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
*    IF sy-subrc = 0.
*      DATA lv_reactiontime TYPE zwr_d_ewmd_reactiontime.
**
**      " Fetch via compliant CDS (not direct table)
*      SELECT SINGLE reaction_time  " aliased field for ZZPOINT_ORIGIN_WDOI
*        FROM zi_wr_ewa_order_object
*        WHERE referenceid = @<lv_refid>
*        INTO @lv_reactiontime.
*      IF sy-subrc = 0.
*        " Map to 1 (red/critical if replanned) or 0 (neutral) — fixed reversed logic
*        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
*        IF sy-subrc = 0.
*          <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
*        ENDIF.
*      ENDIF.
*    ENDIF.
*  ENDLOOP.
ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_TOUR_EXTEND_CALC->IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_REQUESTED_CALC_ELEMENTS     TYPE        TT_ELEMENTS
* | [--->] IV_ENTITY                      TYPE        STRING
* | [<---] ET_REQUESTED_ORIG_ELEMENTS     TYPE        TT_ELEMENTS
* | [!CX!] CX_SADL_EXIT
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
   " Request fields needed for calculations (case-sensitive CDS names)
*    APPEND 'REFERENCEID' TO et_requested_orig_elements.

  endmethod.
ENDCLASS.



@Metadata.layer: #PARTNER

annotate entity /PLCE/C_PDMNLTourWR
    with 
{
    @UI.lineItem: [{ position: 1 },
                 { hidden: true },
                { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Calculate Track', invocationGrouping: #CHANGE_SET, position: 40  },
                 { type:#FOR_ACTION, dataAction:'releaseTour', label:'Tour Freigeben', invocationGrouping: #CHANGE_SET, position: 30  },
                 { type:#FOR_ACTION, dataAction:'recallTour', label:'Tour zurück rufen', invocationGrouping: #CHANGE_SET, position: 25 },
                 { type:#FOR_ACTION, dataAction:'createtour', label:'Tour anlegen', invocationGrouping: #CHANGE_SET, position: 10 },
                 { type:#FOR_ACTION, dataAction:'deleteTour', label:'Tour Löschen', invocationGrouping: #CHANGE_SET, position: 20 },
                 { type:#FOR_ACTION, dataAction:'SequenceAutomated', label: 'Tour automatisch ordnen', position: 50 },
                 { type:#FOR_ACTION, dataAction:'TransferSequence', label:'Servicereihfolge übertragen', invocationGrouping: #CHANGE_SET, position: 70 },
                 { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Calculate Track', invocationGrouping: #CHANGE_SET, qualifier: 'DefaultMap' }
                 
                 
               ]                             
//   
   @UI.hidden: true
    TourUUID; 
 
 @EndUserText.label: 'Restdauer'
 @UI.lineItem: [{ position: 101, importance: #HIGH }]  
 zz_remaining_duration;
 @EndUserText.label: 'Kapazitätsaustung(in Min)'
 @UI.lineItem: [{ position: 102, importance: #HIGH }]  
 tour_capacity_new;
}




@Metadata.layer: #CORE
@UI: {
  headerInfo: {
    typeName: 'Tour',
    typeNamePlural: 'Tours',
    title: {
      type: #STANDARD, value: 'TourId'
    },
    description: {
      type: #STANDARD, value: 'TourTemplateName'
    }
  },
  lineItem: [{ criticality: 'TourCriticality' }],                    //for TourDeviations

  presentationVariant: [{
    sortOrder: [{
      by: 'TourId', direction:  #ASC
    }], visualizations: [{ type: #AS_LINEITEM }]
  },{
    sortOrder: [{
      by: 'TourId', direction:  #ASC
    }], visualizations: [{ type: #AS_LINEITEM, qualifier: 'DefaultMap' }], qualifier: 'DefaultMap'
  }]
}
@Search.searchable: true
annotate entity /PLCE/C_PDMNLTourWR with
{
  @UI.facet: [
  {
    id: 'F0_CoreDefault',
    type: #FIELDGROUP_REFERENCE,
    targetElement: '_TourCore',
    targetQualifier: 'DefaultInformation',
    label: 'Tour Details'
  }]

  @UI.fieldGroup: [{ type: #FOR_ACTION, dataAction: 'releaseTour', label: 'Release Tour', qualifier: 'XXX'  }]

  @UI.lineItem: [{ position: 1 },
                 { hidden: true },
                 { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Calculate Track', invocationGrouping: #CHANGE_SET, position: 30  },
                 { type:#FOR_ACTION, dataAction:'releaseTour', label:'Release Tour', invocationGrouping: #CHANGE_SET, position: 20  },
                 { type:#FOR_ACTION, dataAction:'recallTour', label:'Recall Tour', invocationGrouping: #CHANGE_SET, position: 25 },
                 { type:#FOR_ACTION, dataAction:'deleteTour', label:'Delete Tour', invocationGrouping: #CHANGE_SET, position: 10 },
                 { type:#FOR_ACTION, dataAction:'SequenceAutomated', label: 'Sequence Tour Automatically', position: 40 },
                 { type:#FOR_ACTION, dataAction:'TransferSequence', label:'Transfer Service Sequence', invocationGrouping: #CHANGE_SET, position: 50 },
                 { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Calculate Track', invocationGrouping: #CHANGE_SET, qualifier: 'DefaultMap' }
                ]
  TourUUID;
  @UI.lineItem: [{ position: 10, importance: #HIGH },
                 { position: 10, importance: #HIGH, qualifier: 'DefaultMap' }]
  @UI.fieldGroup: [{position: 10, qualifier: 'DefaultInformation' }]
  TourId;

  @UI.lineItem: [{ position: 20, importance: #HIGH }]
  @UI.textArrangement: #TEXT_SEPARATE
  @UI.fieldGroup: [{position: 20, qualifier: 'DefaultInformation' }]
  TourTemplate;


  @UI.lineItem: [{ position: 30, importance: #HIGH, cssDefault.width: '5rem'  },
                 { position: 20, importance: #HIGH, cssDefault.width: '5rem', qualifier: 'DefaultMap' }]
  WorkStatusIcon;

  @UI.lineItem: [
                 //{ position: 40, criticality: 'TourStatusColorValue', criticalityRepresentation: #WITH_ICON, importance: #HIGH },
                 { position: 30, criticality: 'TourStatusColorValue', criticalityRepresentation: #WITH_ICON, importance: #HIGH, qualifier: 'DefaultMap' }]
  @UI.fieldGroup: [{position: 40, qualifier: 'DefaultInformation' }]
  @UI.textArrangement: #TEXT_ONLY
  TourStatus;
  @UI.lineItem: [{ hidden: true }]
  TourStatusColorValue;
  
  @UI.lineItem: [{ position: 40,  importance: #HIGH, cssDefault.width: '5rem' }]
  TourStatusIcon;

  @UI.selectionField: [{ position: 10}]
  @UI.fieldGroup: [{position: 60, qualifier: 'DefaultInformation' }]
  TourStartDate;

  @UI.lineItem: [{ position: 80 }]
  ScheduledDateTimeStart;

  @UI.lineItem: [{ position: 90 }]
  @UI.fieldGroup: [{position: 130, qualifier: 'DefaultInformation' }]
  MainResource;


  @UI.lineItem: [{ position: 100, importance: #HIGH },
                 { position: 40, importance: #HIGH, qualifier: 'DefaultMap' }]
  @UI.fieldGroup: [{position: 90, qualifier: 'DefaultInformation' }]
  TourDuration;

  @UI.lineItem: [{ position: 110, hidden: true }]
  CalculatedTourDurationUnit;

  @UI.lineItem: [{ position: 110, type : #AS_DATAPOINT, label: 'Capacity used (in %)', importance: #HIGH },
                 { position: 50, type : #AS_DATAPOINT, label: 'Capacity used (in %)', importance: #HIGH, qualifier: 'DefaultMap' }]
  @UI.dataPoint:{
    visualization : #PROGRESS,
    targetValue: 100,
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  TourCapacity;
  @UI.lineItem: [{ hidden: true }]
  TourCapacityColorValue;

  @UI.lineItem: [{ position: 120 },
                 { position: 60, qualifier: 'DefaultMap' }]
  @UI.fieldGroup: [{position: 120, qualifier: 'DefaultInformation' }]
  DrivingTime;
  @UI.lineItem: [{ position: 130 },
                 { position: 70, qualifier: 'DefaultMap' }]
  @UI.fieldGroup: [{position: 130, qualifier: 'DefaultInformation' }]
  IsGeoRouteAvailable;

  @UI.selectionField: [{ element: '_WorkAreaTours.WorkArea', position : 1 }]
  _WorkAreaTours;

  @Search.defaultSearchElement: true
  _Resources;

  @UI.lineItem: [{position: 160, type: #AS_DATAPOINT, label: 'Tonnage capacity (in TO)', importance: #LOW }]
  @UI.dataPoint: { visualization: #PROGRESS, targetValueElement: 'VehicleCapacityInTOSum', criticality: 'CriticalityTonnage' }
  MaterialWeightInTOSum;
  @UI.lineItem: [{ hidden: true }]
  CriticalityTonnage;

  @UI.hidden: true
  TourCriticality;
  @UI.lineItem: [{ position: 170, hidden: true }]
  DeviationsOnTour;
  
  @UI.hidden: true
  StartMapTitle;
  
  @UI.hidden: true
  StartMapPopup;
  
  @UI.hidden: true
  StartMapColor;
  
  @UI.hidden: true
  StartMapHighlight;
  
  @UI.hidden: true
  StartMapSymbol;
  
  @UI.hidden: true
  EndMapTitle;
  
  @UI.hidden: true
  EndMapPopup;
  
  @UI.hidden: true
  EndMapColor;
  
  @UI.hidden: true
  EndMapHighlight;
  
  @UI.hidden: true
  EndMapSymbol;
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




<img width="957" height="756" alt="image" src="https://github.com/user-attachments/assets/5c8ce907-eeee-4070-bb79-03f55d90ddca" />
