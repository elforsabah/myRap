@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Helper CDS for EWA-ORDER_ObJECT'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_WR_EWA_ORDER_OBJECT as select distinct from ewa_order_object as _EWA_ORDER_OBJECT
association [0..1] to /PLCE/R_PDService  as _ServiceCore on _ServiceCore.ReferenceInternalId = $projection.pobjnr

{
    key pobjnr,
    wdplantnr , //Entsorgunganlage,
    zz_pobjnr_main , // Hauptposition
    zz_order_date , //
    _ServiceCore.ReferenceId,
    zz_reactiontime,
    zz_tech_fachbe
} where _ServiceCore.ReferenceId is not initial



extend view entity /PLCE/R_PDServiceExtCustom with
{
  /plce/tpdsrvcst.zz_tech_fachbe,  //Fachbereich
  /plce/tpdsrvcst.zz_discrepancy,  // Termin-Diskrepanz
  /plce/tpdsrvcst.zz_reactiontime, // “Reaktionszeit
  /plce/tpdsrvcst.zz_vehicleinfo,  // Fahrzeuginfo
  /plce/tpdsrvcst.zz_order_date,   // Auftragsdatum
  /plce/tpdsrvcst.zz_timeadjustment, // Zeitanpassung
  /plce/tpdsrvcst.zz_pobjnr_main, // Hauptposition
  /plce/tpdsrvcst.wdplantnr //Entsorgungsanlage 
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
  
//   @ObjectModel.virtualElement: true
//   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
//   @EndUserText.label: 'Hauptposition anzeigen'
//  virtual zz_main_position : zwr_d_pobjnr_main,
  
  @EndUserText.label: 'Hauptposition '
  @UI.selectionField: [{ position: 1000 }]
//  @UI.lineItem: [{ position: 10000]
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  @EndUserText.label: 'Entsorgungsanlage'
   /PLCE/R_PDService._ExtCustom.wdplantnr,
   _TourForRefresh 
}

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

     },
     
          {
       type                : #FOR_ACTION,
       dataAction          : 'anlageaendern',   // bound action name
       label               : 'Anlage Ändern',
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
  @UI.lineItem: [{ position: 135, importance: #HIGH , hidden: true } ]
  zz_main_pos_criticality;
  @EndUserText.label: 'Entsorgungsanlage'
  @UI.lineItem: [{ position: 132, importance: #HIGH } ]
  wdplantnr;
  @UI.lineItem: [{ position: 137, importance: #HIGH , criticality: 'zz_main_pos_criticality'} ]
  zz_pobjnr_main;
   
}


  METHOD if_sadl_exit_calc_element_read~calculate.
    FIELD-SYMBOLS: <ls_original>            TYPE any,
                   <ls_calculated>          TYPE any,
                   <lv_refid>               TYPE any,
                   <lv_service_criticality> TYPE int1,
                   <lv_time_adj>            TYPE any,
                   <lv_entsorgungs>         TYPE any,
                    <lv_entsorgungs_original>         TYPE any,
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
*      ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
      ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
      IF sy-subrc = 0 AND <lv_refid> IS NOT INITIAL.

        CLEAR lv_reactiontime.
        " Fetch via compliant CDS
        SELECT SINGLE * "aliased field for ZZPOINT_ORIGIN_WDOI
          FROM zi_wr_ewa_order_object
          WHERE referenceid = @<lv_refid> INTO @DATA(ls_wr_ewa_order_object).
          lv_reactiontime  = ls_wr_ewa_order_object-zz_reactiontime.

        IF sy-subrc = 0.
          ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
          IF sy-subrc = 0.
            " Map to 1 (red/critical if replanned) or 0 (neutral)
             <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 WHEN ls_wr_ewa_order_object-zz_pobjnr_main IS NOT INITIAL THEN 3 ELSE 0 ).
          ENDIF.

        ENDIF.

      " =======================================================================
        "  Main Position Criticality (Green for Grouped Services)
*
*        ZZ_POBJNR_MAIN IS INITIAL ->Not grouped at all
*        POBJNR = ZZ_POBJNR_MAINMain -> position of a group
*        POBJNR ≠ ZZ_POBJNR_MAIN  -> Sub-position


        ASSIGN COMPONENT 'ZZ_MAIN_POS_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_main_pos_crit>.
        IF sy-subrc = 0.
          IF ls_wr_ewa_order_object-zz_pobjnr_main IS NOT INITIAL.
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


    
