CLASS zcl_wr_tour_extend_calc DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_sadl_exit .
    INTERFACES if_sadl_exit_calc_element_read .
  PROTECTED SECTION.
  PRIVATE SECTION.
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
  METHOD if_sadl_exit_calc_element_read~calculate.
    DATA: lv_capacity_hours TYPE p LENGTH 10 DECIMALS 2,
          lv_total_dur_min  TYPE p LENGTH 10 DECIMALS 2,
          lv_used_min       TYPE p LENGTH 10 DECIMALS 2,
          lv_remaining_min  TYPE p LENGTH 10 DECIMALS 2.

    FIELD-SYMBOLS: <ls_original>   TYPE any,
                   <ls_calculated> TYPE any,
                   <lv_val_hours>  TYPE any,
                   <lv_val_dur>    TYPE any,
                   <lv_set_cap>    TYPE any,
                   <lv_set_rem>    TYPE any.

    LOOP AT it_original_data ASSIGNING <ls_original>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calculated> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " 1. READ Source Fields
      " !!! MATCH THIS NAME WITH THE ONE IN GET_CALCULATION_INFO !!!
      ASSIGN COMPONENT 'TOURCAPACITY' OF STRUCTURE <ls_original> TO <lv_val_hours>.
      ASSIGN COMPONENT 'TOURDURATION'  OF STRUCTURE <ls_original> TO <lv_val_dur>.

      IF <lv_val_hours> IS ASSIGNED AND <lv_val_dur> IS ASSIGNED.
        lv_capacity_hours = <lv_val_hours>.
        lv_total_dur_min  = <lv_val_dur>.
      ELSE.
        CONTINUE.
      ENDIF.

      " 2. CONVERSION & CALCULATION
      " Convert Hours -> Minutes (e.g. 3.83 hours * 60 = 230 minutes)
      lv_used_min = lv_capacity_hours * 60.

      " Calculate Remaining (Total Minutes - Used Minutes)
      IF lv_total_dur_min > 0.
        lv_remaining_min = lv_total_dur_min - lv_used_min.
      ELSE.
        lv_remaining_min = 0.
      ENDIF.

      " 3. ASSIGN to Virtual Fields

      " Set tour_capacity_new (Display as 230)
      ASSIGN COMPONENT 'TOUR_CAPACITY_NEW' OF STRUCTURE <ls_calculated> TO <lv_set_cap>.
      IF sy-subrc = 0.
        <lv_set_cap> = lv_used_min.
      ENDIF.

      " Set zz_remaining_duration (Display as 280)
      ASSIGN COMPONENT 'ZZ_REMAINING_DURATION' OF STRUCTURE <ls_calculated> TO <lv_set_rem>.
      IF sy-subrc = 0.
        <lv_set_rem> = lv_remaining_min.
      ENDIF.

    ENDLOOP.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_TOUR_EXTEND_CALC->IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_REQUESTED_CALC_ELEMENTS     TYPE        TT_ELEMENTS
* | [--->] IV_ENTITY                      TYPE        STRING
* | [<---] ET_REQUESTED_ORIG_ELEMENTS     TYPE        TT_ELEMENTS
* | [!CX!] CX_SADL_EXIT
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD if_sadl_exit_calc_element_read~get_calculation_info..
  " 1. Request the source fields needed for calculation
  "    Note: Field names must match the CDS view Element names exactly (Case Sensitive)
  APPEND 'TOURCAPACITY'  TO  et_requested_orig_elements.
*  APPEND 'REMAININGTIME' TO  et_requested_orig_elements.
  APPEND 'TOURDURATION'  TO  et_requested_orig_elements.
ENDMETHOD.
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
 
    // Remaining Duration (e.g. 280)
  @EndUserText.label: 'Restdauer(freie Kapazität)'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]  
  zz_remaining_duration;
 
  // Capacity Used (e.g. 230)
  @EndUserText.label: 'Kapazitätsauslastung in Min '
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Kapazitätsauslastung in Min', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Kapazitätsauslastung in Min', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint: {
    visualization: #PROGRESS,
    // Compares 230 (Used Min) vs 510 (Total Min)
    targetValueElement: 'TourDuration', 
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;

}



extend view entity /PLCE/C_PDMNLTourWR with {
   /PLCE/R_PDTour._ExtCustom.zz_remaining_duration, // Restdauer
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_TOUR_EXTEND_CALC'  // Reference your new class
   virtual tour_capacity_new : abap.char( 45 )
   
}


extend view entity /PLCE/R_PDTourExtCustom with {
   /plce/tpdtourcst.zz_remaining_duration
}


