3.2.	Im P&D – EA eines Auftrages ändern 
In der Dispo im Planungscockpit (P&D) werden die Entsorgungsanlagen mit den Bezeichnungen aus EWAEL04 eingeblendet und eine neue Änderungsfunktion der Service-Liste umgesetzt:
o	Eingeschränkt / nicht erlaubt, wenn für den Auftrag ein fester Entsorgungsvertrag vorgegeben ist (Fehlermeldung).
o	Zur Auswahl stehen nur Entsorgungsanlagen mit dem Kennzeichen „verfügbar für Dispo“ (s.o.) und es findet ein Abgleich des AVV-Codes mit der Abfallliste zur Anlage (s.o.) statt.
o	Wenn an einer Anlage keine AVV-Zuordnungen gepflegt sind, sind dort prinzipiell alle Abfälle erlaubt. KVS möchte dies für Fremdanlagen allgemein vornehmen und muss sicherstellen, dass die Abfälle und entsprechende Positionen im zugehörigen Rahmenvertrag vorhanden sind.

CLASS lhc_service DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS get_global_features FOR GLOBAL FEATURES
      IMPORTING REQUEST requested_features FOR service RESULT result.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR service RESULT result.

    METHODS assignworkarea FOR MODIFY
      IMPORTING keys FOR ACTION service~assignworkarea RESULT result.

    METHODS precheck_assignworkarea FOR PRECHECK
      IMPORTING keys FOR ACTION service~assignworkarea.

    METHODS setfachbereich FOR DETERMINE ON MODIFY
      IMPORTING keys FOR service~setfachbereich.
    METHODS adjusttime FOR MODIFY
      IMPORTING keys FOR ACTION service~adjusttime RESULT result.
    METHODS precheck_adjusttime FOR PRECHECK
      IMPORTING keys FOR ACTION service~adjusttime.
    METHODS terminateservice FOR MODIFY
      IMPORTING keys FOR ACTION service~terminateservice RESULT result.

    METHODS precheck_terminateservice FOR PRECHECK
      IMPORTING keys FOR ACTION service~terminateservice.
    METHODS anlageaendern FOR MODIFY
      IMPORTING keys FOR ACTION service~anlageaendern RESULT result.

    METHODS precheck_anlageaendern FOR PRECHECK
      IMPORTING keys FOR ACTION service~anlageaendern.

ENDCLASS.

CLASS lhc_service IMPLEMENTATION.

  METHOD setfachbereich.

    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
         ENTITY service
         FIELDS ( referenceid serviceuuid ) " // Use CDS casing; add ServiceUUID for key
         WITH CORRESPONDING #( keys )
         RESULT DATA(lt_services)
         FAILED DATA(lt_failed).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA lv_zz_tech_fachbe TYPE ewa_order_object .
      SELECT SINGLE zz_tech_fachbe
        FROM ewa_order_object
        WHERE pobjnr = @<ls_service>-referenceinternalid
        INTO @lv_zz_tech_fachbe.

      IF sy-subrc = 0 AND lv_zz_tech_fachbe IS NOT INITIAL.
*     " // Check if ExtCustom exists; read it first
        READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY extcustom
          FIELDS ( serviceuuid )  "// Minimal to check existence
          WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
          RESULT DATA(lt_extcustom).

        IF lt_extcustom IS INITIAL.
*        // Create if missing
          DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
                lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.
          APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
          <ls_ext_create>-serviceuuid = <ls_service>-serviceuuid. " // %tky or key
          <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

          MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
            ENTITY service
            CREATE BY \_extcustom
            AUTO FILL CID SET FIELDS WITH lt_ext_create
            MAPPED DATA(lmapped)
            FAILED DATA(lfailed)
            REPORTED DATA(lreported).
*        // Handle errors if needed
        ENDIF.

*      // Now update the field
        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY extcustom
          UPDATE FIELDS ( zz_tech_fachbe )
          WITH VALUE #( ( %key-serviceuuid = <ls_service>-serviceuuid
                          zz_tech_fachbe = lv_zz_tech_fachbe ) )
          FAILED DATA(lt_failed_update)
          REPORTED DATA(lt_reported_update).

        " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
        READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
            ALL FIELDS WITH CORRESPONDING #( keys )
          RESULT DATA(lt_service_result).

        CLEAR: lt_ext_create,  lt_reported_update,  lt_failed_update,  lreported, lfailed, lmapped.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

  METHOD get_global_features.
  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD assignworkarea.

    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service
    FIELDS ( referenceid serviceuuid ) " // Use CDS casing; add ServiceUUID for key
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services)
    FAILED DATA(lt_failed).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_service>).
*     " // Check if ExtCustom exists; read it first
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        FIELDS ( serviceuuid )  "// Minimal to check existence
        WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
*        // Create if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
              lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.
        APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
        <ls_ext_create>-serviceuuid = <ls_service>-serviceuuid. " // %tky or key
        <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
          CREATE BY \_extcustom
          AUTO FILL CID SET FIELDS WITH lt_ext_create
          MAPPED DATA(lmapped)
          FAILED DATA(lfailed)
          REPORTED DATA(lreported).
*        // Handle errors if needed
      ENDIF.

*      // Now update the field
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        UPDATE FIELDS ( zz_tech_fachbe )
        WITH VALUE #( ( %key-serviceuuid = <ls_service>-serviceuuid
                        zz_tech_fachbe = <ls_service>-%param-workarea ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).


      " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      result = VALUE #( FOR ls_service IN lt_service_result ( %tky   = ls_service-%tky
                                                             %param  = ls_service ) ).
      CLEAR:   lt_ext_create,  lt_reported_update,  lt_failed_update,  lreported, lfailed, lmapped.
    ENDLOOP.
  ENDMETHOD.

  METHOD precheck_assignworkarea.

    LOOP AT keys INTO DATA(ls_service).
*      " Check if WorkArea is valid (query value help entity if needed).
*      IF ls_service-%param-WorkArea IS INITIAL.
*        APPEND VALUE #( %key = ls_service-%key
*                        %msg = new_message( id = 'Z_MSG_CLASS' number = '000' severity = if_abap_behv_message=>severity-error ) )
*               TO reported-Service.
*      ENDIF.

    ENDLOOP.
  ENDMETHOD.

  METHOD adjusttime.
    " 1. Loop through the keys (Service Instances)
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 2. Check if Extension Record exists
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        FIELDS ( serviceuuid )
        WITH VALUE #( ( serviceuuid = <ls_key>-serviceuuid ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
        " 3. Create Extension if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid_adj_',
              lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom.

        APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
        <ls_ext_create>-serviceuuid = <ls_key>-serviceuuid.
        <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

        MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
          ENTITY service
          CREATE BY \_extcustom
          AUTO FILL CID SET FIELDS WITH lt_ext_create
          MAPPED DATA(lmapped_create)
          FAILED DATA(lfailed_create)
          REPORTED DATA(lreported_create).

        " Optional: Handle creation errors here if needed
      ENDIF.

      " ====================================================================
      " 4. Prepare the Numeric Value
      " ====================================================================
      DATA(lv_raw_input) = <ls_key>-%param-zeitinmin.

      " Assuming your DB field is now an INT4. If it's a DEC, change TYPE i to TYPE p.
      DATA lv_numeric_result TYPE i.

      " Normalize input (replace comma with dot if user typed "37,5")
      DATA lv_calc_val TYPE string.
      lv_calc_val = lv_raw_input.
      REPLACE ALL OCCURRENCES OF ',' IN lv_calc_val WITH '.'.
      CONDENSE lv_calc_val NO-GAPS.

      " Safely convert the input to a pure number
      TRY.
          lv_numeric_result = CONV i( lv_calc_val ).
        CATCH cx_sy_conversion_no_number.
          " Fallback if the user typed garbage text instead of a number
          lv_numeric_result = 0.
      ENDTRY.

      " ====================================================================
      " 5. Update the field with
      " ====================================================================
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        UPDATE FIELDS ( zz_timeadjustment )
        WITH VALUE #( ( %key-serviceuuid = <ls_key>-serviceuuid
                        zz_timeadjustment = lv_numeric_result ) ) " <--- Pure Number
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      " 6. Aggregate errors
      APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
      APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.


      " 7. Read Result to refresh UI
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
        ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      LOOP AT lt_service_result ASSIGNING FIELD-SYMBOL(<ls_result>).
        INSERT VALUE #( %tky   = <ls_result>-%tky
                        %param = <ls_result> ) INTO TABLE result.
      ENDLOOP.

    ENDLOOP.

  ENDMETHOD.


  METHOD precheck_adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " ---------------------------------------------------------------------
      " 1. Parse Input & Format Validation
      " ---------------------------------------------------------------------
      DATA(lv_input_str) = CONV string( <ls_key>-%param-zeitinmin ).
      REPLACE ALL OCCURRENCES OF ',' IN lv_input_str WITH '.'.
      CONDENSE lv_input_str NO-GAPS.

      " Regex: Allow integers or decimals, optional minus sign
      IF lv_input_str IS NOT INITIAL.
        IF NOT matches( val = lv_input_str regex = '^-?[0-9]+([.,][0-9]+)?$' ).
          APPEND VALUE #( %tky = <ls_key>-%tky
                          %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
          APPEND VALUE #( %tky = <ls_key>-%tky

                           %msg = new_message(
                       id       = 'Z_MSG_SVR_TOUR_EXT'
                       number   = '005'
                       severity = if_abap_behv_message=>severity-error

                     )

                         ) TO reported-service.
          CONTINUE.
        ENDIF.
      ENDIF.

      " Convert to Number for calculations
      DATA lv_adj_min  TYPE decfloat34.
      TRY.
          lv_adj_min = lv_input_str.
        CATCH cx_sy_conversion_no_number.
          CONTINUE.
      ENDTRY.

      " ---------------------------------------------------------------------
      " 2. Limit Check: Max +/- 600 Minutes (10 Hours)
      " ---------------------------------------------------------------------
      IF lv_adj_min > 600 OR lv_adj_min < -600.
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_key>-%tky
                           %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '006'
                     severity = if_abap_behv_message=>severity-error

                   )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

METHOD terminateservice.
    DATA: lt_unassign_keys  TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour\\serviceassignment~unassign,
          lt_service_update TYPE TABLE FOR UPDATE /plce/r_pdservice,
          lt_return         TYPE bapiret2_t.

    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid ReferenceId servicestatus serviceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.
      CLEAR lt_return.

      " 1. Do the Waste Order Storno/Delete in the Backend
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-referenceinternalid )
          iv_reason_predefined = 'PLSTORNO'
          iv_reason_text       = CONV string( ls_param-stornogrund )
          iv_serviceuuid       = <ls_service>-serviceuuid
        IMPORTING
          et_return            = lt_return.

      IF lt_return IS INITIAL.
        " --- SUCCESS ---

        " A. Update Status to '01' to keep Fiori happy for the next 3 seconds
        APPEND VALUE #( %tky = <ls_service>-%tky
                        servicestatus = '01' ) TO lt_service_update.

        " C. Prepare Tour Unassignment
        SELECT SINGLE asgmt~serviceuuid, asgmt~touruuid
          FROM /plce/r_pdtourserviceasgmt AS asgmt
          WHERE asgmt~serviceuuid = @<ls_service>-serviceuuid
          INTO @DATA(ls_unassign).

        IF sy-subrc = 0.
          APPEND VALUE #( %key-touruuid    = ls_unassign-touruuid
                          %key-serviceuuid = ls_unassign-serviceuuid ) TO lt_unassign_keys.
        ENDIF.

        " D. Success Message
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                            number   = '009'
                                            severity = if_abap_behv_message=>severity-information
                                            v1       = |{ <ls_service>-ReferenceId ALPHA = OUT }|  )
                      ) TO reported-service.

      ELSE.
        " --- FAILURE ---
        APPEND VALUE #( %tky = <ls_service>-%tky ) TO failed-service.

        LOOP AT lt_return INTO DATA(ls_ret).
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message_with_text(
                                   severity = if_abap_behv_message=>severity-error
                                   text     = CONV #( ls_ret-message ) )
                        ) TO reported-service.
        ENDLOOP.
      ENDIF.
    ENDLOOP.

    " 2. Bulk Modify RAP Entities (Just the temporary status update)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          UPDATE FIELDS ( servicestatus ) WITH lt_service_update.
    ENDIF.

    " 3. Bulk Execute Tour Unassignment
    IF lt_unassign_keys IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdtour
        ENTITY serviceassignment
          EXECUTE unassign FROM lt_unassign_keys
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).
    ENDIF.

    " 4. Return the updated data to the UI
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.


  METHOD precheck_terminateservice.
  ENDMETHOD.


  METHOD anlageaendern.
  ENDMETHOD.

  METHOD precheck_anlageaendern.
  ENDMETHOD.

ENDCLASS.


extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend  behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
action ( precheck, features : global ) adjusttime parameter ZAE_D_adjusttime_AB result [0..*] $self;

action ( precheck, features : global ) anlageaendern parameter ZAE_D_ANLAGE_AENDERN_AB result [0..*] $self;



 action ( precheck, features : global ) terminateservice
  parameter ZAE_D_TERMINATE_SERVICE
  result [0..*] $self;


 determination Setfachbereich on modify { create; update; }


 side effects {

 action adjusttime affects $self, entity _ExtCustom;
 action assignworkarea affects $self;
 action terminateservice affects $self;

 }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
    zz_vehicleinfo = zz_vehicleinfo;
    zz_discrepancy = zz_discrepancy;
    zz_reactiontime =  zz_reactiontime;
    zz_order_date = zz_order_date;
    zz_timeadjustment = zz_timeadjustment;
 }

field ( readonly : update ) zz_tech_fachbe;
field ( readonly : update ) zz_vehicleinfo;
field ( readonly : update ) zz_discrepancy;
field ( readonly : update ) zz_reactiontime;
field ( readonly : update ) zz_order_date;
field ( readonly : update ) zz_timeadjustment;
}


@EndUserText.label: 'Abstract View für Entsorgungsanlage'

define abstract entity ZAE_D_ANLAGE_AENDERN_AB
 
{
  @EndUserText.label: 'Entsorgungsanlage'
 @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZAE_D_ENTSORGUNG_ANLAGE_VH', element: 'Entsorgungsanlage' },
      label          : 'Entsorgungsanlage'
    }
  ]
   anlage : ewael_wdplantnr; 
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Value Help für Entsorgungsanlage'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@ObjectModel.resultSet.sizeCategory: #XS
define view entity ZAE_D_ENTSORGUNG_ANLAGE_VH as select from ewa_el_wdplant
{
        
         @EndUserText.label: 'Entsorgungsanlage'
        key wdplantnr as Entsorgungsanlage     
}
  

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
*            <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
             <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 WHEN ls_wr_ewa_order_object-main_position IS NOT INITIAL THEN 3 ELSE 0 ).
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

extend view entity /PLCE/R_PDServiceExtCustom with 
 {
/plce/tpdsrvcst.zz_tech_fachbe,  //Fachbereich
/plce/tpdsrvcst.zz_discrepancy,  // Termin-Diskrepanz
/plce/tpdsrvcst.zz_reactiontime, // “Reaktionszeit
/plce/tpdsrvcst.zz_vehicleinfo,  // Fahrzeuginfo
/plce/tpdsrvcst.zz_order_date,   // Auftragsdatum
/plce/tpdsrvcst.zz_timeadjustment, // Zeitanpassung
/plce/tpdsrvcst.zz_pobjnr_main,  // Hauptposition
/plce/tpdsrvcst.zz_ent_anlage  // Entsorgungsanlage
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
  
   @UI.hidden: true
   /PLCE/R_PDService._ExtCustom.zz_ent_anlage as entsorgungsanlage,
  
  
   _TourForRefresh 
}

    METHOD if_sadl_exit_calc_element_read~get_calculation_info.
      APPEND 'REFERENCEID' TO et_requested_orig_elements.
      APPEND 'ZZ_TIMEADJUSTMENT' TO et_requested_orig_elements.
    ENDMETHOD.
ENDCLASS.



<img width="1219" height="653" alt="image" src="https://github.com/user-attachments/assets/ab1c1a3a-3484-4c09-af75-bb5717360eff" />
