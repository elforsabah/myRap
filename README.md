@Metadata.layer: #PARTNER
annotate entity /PLCE/C_PDMNLTourWR with
{
  // ... (Previous annotations) ...

  @EndUserText.label: 'Restdauer'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]  
  @UI.fieldGroup: [{ position: 101, qualifier: 'DefaultInformation' }]
  zz_remaining_duration;
 
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Kapazitätsauslastung', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Kapazitätsauslastung', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint:{
    visualization : #PROGRESS,
    // CRITICAL CHANGE: Use targetValueElement instead of fixed targetValue
    // This compares tour_capacity_new (230) against TourDuration (510)
    targetValueElement: 'TourDuration', 
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;
}




CLASS ZCL_WR_TOUR_EXTEND_CALC IMPLEMENTATION.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
    " Request the source fields needed for calculation
    INSERT 'TourCapacity' INTO TABLE et_requested_orig_elements.
    INSERT 'TourDuration' INTO TABLE et_requested_orig_elements.
  ENDMETHOD.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
    DATA: lv_tour_capacity_pct TYPE p LENGTH 5 DECIMALS 2,
          lv_tour_duration     TYPE p LENGTH 10 DECIMALS 2,
          lv_used_duration     TYPE p LENGTH 10 DECIMALS 2,
          lv_remaining         TYPE p LENGTH 10 DECIMALS 2.

    FIELD-SYMBOLS: <ls_original>   TYPE any,
                   <ls_calculated> TYPE any,
                   <lv_val_cap>    TYPE any,
                   <lv_val_dur>    TYPE any,
                   <lv_set_cap>    TYPE any,
                   <lv_set_rem>    TYPE any.

    LOOP AT it_original_data ASSIGNING <ls_original>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calculated> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      ASSIGN COMPONENT 'TourCapacity' OF STRUCTURE <ls_original> TO <lv_val_cap>.
      ASSIGN COMPONENT 'TourDuration' OF STRUCTURE <ls_original> TO <lv_val_dur>.

      IF <lv_val_cap> IS ASSIGNED AND <lv_val_dur> IS ASSIGNED.
        lv_tour_capacity_pct = <lv_val_cap>.
        lv_tour_duration     = <lv_val_dur>.
      ELSE.
        CONTINUE.
      ENDIF.

      " --- CALCULATION LOGIC ---
      IF lv_tour_duration IS NOT INITIAL.
        " 1. Calculate USED Duration (e.g., 510 * 45% = 229.5 -> 230)
        lv_used_duration = ( lv_tour_duration * lv_tour_capacity_pct ) / 100.
        
        " 2. Calculate REMAINING Duration (e.g., 510 - 230 = 280)
        lv_remaining = lv_tour_duration - lv_used_duration.
      ELSE.
        lv_used_duration = 0.
        lv_remaining = 0.
      ENDIF.

      " --- ASSIGNMENT ---
      
      " Set tour_capacity_new to the Absolute Used Time (230)
      ASSIGN COMPONENT 'tour_capacity_new' OF STRUCTURE <ls_calculated> TO <lv_set_cap>.
      IF sy-subrc = 0.
        <lv_set_cap> = lv_used_duration. 
      ENDIF.

      " Set zz_remaining_duration (280)
      ASSIGN COMPONENT 'zz_remaining_duration' OF STRUCTURE <ls_calculated> TO <lv_set_rem>.
      IF sy-subrc = 0.
        <lv_set_rem> = lv_remaining.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
