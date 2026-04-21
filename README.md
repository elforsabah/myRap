CLASS zcl_wr_service_extend_calc DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_sadl_exit .
    INTERFACES if_sadl_exit_calc_element_read .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS zcl_wr_service_extend_calc IMPLEMENTATION.


  METHOD if_sadl_exit_calc_element_read~calculate.
    FIELD-SYMBOLS: <ls_original>            TYPE any,
                   <ls_calculated>          TYPE any,
                   <lv_refid>               TYPE any,
                   <lv_service_criticality> TYPE int1,
                   <lv_time_adj>            TYPE any,
                   <lv_entsorgungs>         TYPE any,
                   <lv_time_disp>           TYPE any,
                   <lv_main_position>       TYPE any,
                   <lv_main_pos_crit>       TYPE int1.

    DATA: lv_reactiontime TYPE zwr_d_ewmd_reactiontime,
          lv_time_num     TYPE i,
          lv_time_str     TYPE string.

*   ct_calculated_data = CORRESPONDING #(  it_original_data ).
    clear ct_calculated_data.
    LOOP AT it_original_data ASSIGNING <ls_original>.
      " Prepare the calculated row
       APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
       MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

      " =======================================================================
      " Service Criticality
      " =======================================================================
      ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
      IF sy-subrc = 0 AND <lv_refid> IS NOT INITIAL.

        CLEAR lv_reactiontime.
        " Fetch via compliant CDS
        SELECT SINGLE * "aliased field for ZZPOINT_ORIGIN_WDOI
          FROM zi_wr_ewa_order_object
          WHERE referenceid = @<lv_refid> INTO @DATA(ls_wr_ewa_order_object).
        lv_reactiontime  = ls_wr_ewa_order_object-reaction_time.

        DATA(lv_entsorgungsanlage) = ls_wr_ewa_order_object-entsorgunganlage.

        IF sy-subrc = 0.
          ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
          IF sy-subrc = 0.
            " Map to 1 (red/critical if replanned) or 0 (neutral)
            <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
          ENDIF.

          ASSIGN COMPONENT 'ZZ_ENTSORGUNGANLAGE' OF STRUCTURE <ls_calculated> TO <lv_entsorgungs>.
          IF sy-subrc = 0.
            <lv_entsorgungs> = ls_wr_ewa_order_object-entsorgunganlage.
          ENDIF.
        ENDIF.

      " =======================================================================
        "  Main Position Criticality (Green for Grouped Services)
*
*        ZZ_POBJNR_MAIN IS INITIAL ->Not grouped at all
*        POBJNR = ZZ_POBJNR_MAINMain -> position of a group
*        POBJNR ≠ ZZ_POBJNR_MAIN  -> Sub-position
*
        " =======================================================================
        ASSIGN COMPONENT 'ZZ_MAIN_POSITION' OF STRUCTURE <ls_calculated> TO <lv_main_position>.
        IF sy-subrc = 0.
*          " Fill display field with converted number
          <lv_main_position> = zcl_wr_eewa_wdoc_misc=>cl_convert_objnr_out(
                                 par_pobjnr = ls_wr_ewa_order_object-main_position ).
        ENDIF.

        ASSIGN COMPONENT 'ZZ_MAIN_POS_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_main_pos_crit>.
        IF sy-subrc = 0.
          IF ls_wr_ewa_order_object-main_position IS NOT INITIAL.
            " Belongs to a group (main or sub) -> Green
            <lv_main_pos_crit> = 3.
          ELSE.
            " No group -> Neutral
            <lv_main_pos_crit> = 0.
          ENDIF.
        ENDIF.

        " =======================================================================
        " Time Adjustment Formatting (+37 MIN)
        " =======================================================================
        ASSIGN COMPONENT 'ZZ_TIMEADJUSTMENT' OF STRUCTURE <ls_original> TO <lv_time_adj>.
        IF sy-subrc = 0 AND <lv_time_adj> IS NOT INITIAL.
          lv_time_num = <lv_time_adj>.
        ELSE.
          lv_time_num = 0.
        ENDIF.

        " Format the string based on positive, negative, or zero
        IF lv_time_num > 0.
          lv_time_str = |+{ lv_time_num } MIN|.
        ELSEIF lv_time_num < 0.
          " Negative sign is automatically included in the variable, so just append ' MIN'
          lv_time_str = |{ lv_time_num } MIN|.
        ELSE.
          lv_time_str = |0 MIN|.
        ENDIF.

        " Assign the formatted string to the Virtual Display Field
        ASSIGN COMPONENT 'ZZ_TIMEADJUST_DISPLAY' OF STRUCTURE <ls_calculated> TO <lv_time_disp>.
        IF sy-subrc = 0.
          <lv_time_disp> = lv_time_str.
        ENDIF.


      ENDIF.
      ENDLOOP.
    ENDMETHOD.


    METHOD if_sadl_exit_calc_element_read~get_calculation_info.
      APPEND 'REFERENCEID' TO et_requested_orig_elements.
      APPEND 'ZZ_TIMEADJUSTMENT' TO et_requested_orig_elements.
    ENDMETHOD.
ENDCLASS.

@Metadata.layer: #CUSTOMER
@UI: {
lineItem: [{ criticality: 'service_criticality' }]
}
annotate entity /PLCE/C_PDMNLServiceWR with

{
  @UI.lineItem: [
     { position: 190, importance: #HIGH },
     {
       type                : #FOR_ACTION,
       dataAction          : 'assignworkarea',   // bound action name
       label               : 'Arbeitsbereich zuweisen',
       iconUrl: 'sap-icon://wrench',
  //      requiresSelection   : false,              // => global action
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH

     },
     
     { 
       type                : #FOR_ACTION,
       dataAction          : 'adjusttime',   // bound action name
       label               : 'Zeitanpassung ändern',
       iconUrl: 'sap-icon://wrench',
//        requiresSelection   : false,              // => global action
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH
       
     }, 
     
     {
       type                : #FOR_ACTION,
       dataAction          : 'terminateservice',   // bound action name
       label               : 'Service stornieren',
       iconUrl: 'sap-icon://wrench',
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH

     }
     
   ]
   
  
  @EndUserText.label: 'Fachbereich'
  @UI.selectionField: [{position: 20}]
  @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZZPD_WORKREA_VH', element: 'WorkArea' },
      label          : 'Workarea',
      useForValidation: true
    }
  ]
  zz_tech_fachbe;
  @EndUserText.label: 'Termin-Diskrepanz'
  zz_discrepancy; // Termin-Diskrepanz
  @EndUserText.label: 'Reaktionszeit'
  zz_reactiontime; // “Reaktionszeit
  @EndUserText.label: 'Fahrzeuginfo'
  zz_vehicleinfo;  // Fahrzeuginfo
  @EndUserText.label: 'Zeitanpassung in Min'
  @UI.lineItem: [{ position: 131, importance: #HIGH } ]
  zz_timeadjust_display; // Zeitanpassung
  @UI.hidden: true
  service_criticality;
//  @UI.hidden: true
  @EndUserText.label: 'main Criticallity'
  @UI.lineItem: [{ position: 135, importance: #HIGH } ]
  zz_main_pos_criticality;
  @EndUserText.label: 'Entsorgungsanlage'
  @UI.lineItem: [{ position: 132, importance: #HIGH } ]
  zz_entsorgunganlage;
  @UI.lineItem: [{ position: 137, importance: #HIGH } ]
  zz_main_position;
}

extend view entity /PLCE/C_PDMNLServiceWR with

 // 1. DEFINE the Association (The Bridge)
   association [0..1] to /PLCE/C_PDMNLTourWR as _TourForRefresh 
      on $projection.TourId = _TourForRefresh.TourId
 {

   /PLCE/R_PDService._ExtCustom.zz_tech_fachbe, //as Fachberei
   /PLCE/R_PDService._ExtCustom.zz_discrepancy,  // Termin-Diskrepanz
   
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
   virtual service_criticality : abap.int1,
   /PLCE/R_PDService._ExtCustom.zz_reactiontime, // “Reaktionszeit
   /PLCE/R_PDService._ExtCustom.zz_vehicleinfo,  // Fahrzeuginfo
   @UI.hidden: true
   /PLCE/R_PDService._ExtCustom.zz_timeadjustment, // Zeitanpassung
   
   // 2. The NEW VIRTUAL field (Formatted Text). 
   // This will hold the "+37 MIN" string for the UI.
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
   @EndUserText.label: 'Zeitanpassung in Min'
   virtual zz_timeadjust_display : abap.char( 35 ),
   
  @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition Kritikalität'
  virtual zz_main_pos_criticality : abap.int1,
  
   @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition'
  virtual zz_main_position : zwr_d_pobjnr_main,
  
  @EndUserText.label: 'Hauptposition '
//  @UI.selectionField: [{ position: 1000 }]
   @UI.lineItem: [{ hidden: true }]
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  virtual zz_entsorgunganlage :  ewael_wdplantnr, // Entsorgungsanlage
  
   _TourForRefresh 
}





<img width="1123" height="1005" alt="image" src="https://github.com/user-attachments/assets/839a029d-1603-4816-8529-7c8e12076a66" />

