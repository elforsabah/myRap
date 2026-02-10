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
 
  @UI.lineItem: [{ position: 102, type : #AS_DATAPOINT, label: 'Capacity used (in %)', importance: #HIGH },
                 { position: 50, type : #AS_DATAPOINT, label: 'Capacity used (in %)', importance: #HIGH, qualifier: 'DefaultMap' }]
  @UI.dataPoint:{
    visualization : #PROGRESS,
    targetValue: 100,
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
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


METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
  FIELD-SYMBOLS: <ls_original>            TYPE any,
                 <ls_calculated>          TYPE any,
                 <lv_refid>               TYPE any,
*                 <lv_service_criticality> TYPE int1,
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


  method IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
   " Request fields needed for calculations (case-sensitive CDS names)
*    APPEND 'REFERENCEID' TO et_requested_orig_elements.

  endmethod.
ENDCLASS.
