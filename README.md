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
                       id       = 'Z_MSG_CL_SERVICE_EXT'
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
                     id       = 'Z_MSG_CL_SERVICE_EXT'
                     number   = '006'
                     severity = if_abap_behv_message=>severity-error

                   )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

METHOD terminateService.
    DATA: assigned_services TYPE TABLE FOR ACTION IMPORT /PLCE/R_PDTour\\ServiceAssignment~UnAssign,
          lt_service_update TYPE TABLE FOR UPDATE /PLCE/R_PDService,
          lt_history_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_StatusHistory.

    " 1. Read the service to get POBJNR (ReferenceInternalId)
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      FIELDS ( ServiceUUID ReferenceInternalId ServiceStatus ServiceId )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " 2. Unassign from Tour (if it is currently planned)
    SELECT asgmt~ServiceUUID, asgmt~TourUUID
      FROM /PLCE/R_PDTourServiceAsgmt AS asgmt
      INNER JOIN @keys AS keys ON asgmt~ServiceUUID = keys~ServiceUUID
      INTO CORRESPONDING FIELDS OF TABLE @assigned_services.

    IF assigned_services IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDTour
        ENTITY ServiceAssignment
          EXECUTE UnAssign FROM assigned_services
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).

      " Pass any unassignment messages to the UI
      /plce/cl_base_misc=>check_response(
        EXPORTING is_response = srvc_unassign_reported
        CHANGING  ct_messages = reported-%other ).
    ENDIF.

    " 3. Process the Storno for each selected service
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      " Extract the input from your UI Popup
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

      TRY.
          " Call Reusable Waste Order Logic
          zcl_wr_waste_order_api=>cancel_waste_order_item(
            iv_pobjnr      = <ls_service>-ReferenceInternalId
            iv_reason_predefined = ls_param-definiertegrund
            iv_reason_text = ls_param-stornogrund
          ).

          " Add to Bulk Update tables instead of modifying inside the loop
          " NOTE: Replace 'CANC' with your actual constant for Cancelled/Storno
          APPEND VALUE #( %tky = <ls_service>-%tky
                          ServiceStatus = 'CANC' ) TO lt_service_update.

          APPEND VALUE #( %tky = <ls_service>-%tky
                          %target = VALUE #( ( %cid = |HIST_{ sy-tabix }|
                                               ServiceStatus = 'CANC' ) ) ) TO lt_history_create.

          " Success Message
          APPEND NEW /plce/cx_pd_exception(
                       severity = if_abap_behv_message=>severity-success
                       textid   = /plce/cx_pd_exception=>service_assigned
                       serviceid = <ls_service>-ServiceId ) TO reported-%other.

        CATCH cx_eewa_base INTO DATA(lo_eewa_ex).
          " Safely map the legacy IS-U Exception to a RAP Message
          APPEND VALUE #( %tky = <ls_service>-%tky ) TO failed-service.

          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message_with_text(
                                   severity = if_abap_behv_message=>severity-error
                                   text     = lo_eewa_ex->get_text( ) )
                        ) TO reported-service.
      ENDTRY.
    ENDLOOP.

    " 4. Bulk Modify Entities (Performance Optimization)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
          UPDATE FIELDS ( ServiceStatus )
          WITH lt_service_update
          CREATE BY \_StatusHistory
          SET FIELDS WITH lt_history_create.
    ENDIF.

    " 5. Return the updated data to the UI
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.

  METHOD precheck_terminateservice.
  ENDMETHOD.

ENDCLASS.





class ZCL_WR_WASTE_ORDER_API definition
  public
  inheriting from /PLCP/CL_TA_WA_ORDER_RSLT "Inherit to access GETBO & SAVE_BO
  final
  create public .

public section.

    " Static wrapper for easy calling from RAP / Interaction Center
  class-methods CANCEL_WASTE_ORDER_ITEM
    importing
      !IV_POBJNR type CHAR30
      !IV_REASON_PREDEFINED type CHAR255
      !IV_REASON_TEXT type STRING
    raising
      CX_EEWA_BASE .
private section.

    " Instance method to actually execute the logic using inherited methods
  methods EXECUTE_CANCEL
    importing
      !IV_POBJNR type CHAR30
      !IV_REASON_PREDEFINED type CHAR255
      !IV_REASON_TEXT type STRING
    raising
      CX_EEWA_BASE .
ENDCLASS.



CLASS ZCL_WR_WASTE_ORDER_API IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Private Method ZCL_WR_WASTE_ORDER_API->EXECUTE_CANCEL
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_POBJNR                      TYPE        CHAR30
* | [--->] IV_REASON_PREDEFINED           TYPE        CHAR255
* | [--->] IV_REASON_TEXT                 TYPE        STRING
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD execute_cancel.
    DATA: ls_item_key TYPE ewa_order_object_ikey,
          lo_bo_item  TYPE REF TO zcl_wr_eewa_bo_wdorderitem.

    " 1. Find the Order Item keys based on POBJNR
    SELECT SINGLE ordernr, order_laufnr
      FROM ewa_order_object
      INTO @ls_item_key
      WHERE pobjnr = @iv_pobjnr.

    CHECK sy-subrc = 0.

    " 2. Instantiate the IS-U Waste Order Item BO using inherited GETBO
    lo_bo_item ?= getbo(
        par_objtype = cl_eewa_bo_wdorderitem=>cot_wdorderitem
        par_key     = ls_item_key
        par_lock    = 'X' ).

    " 3. Apply the Storno Updates to the BO Data Reference
    lo_bo_item->dataref->conftype1 = iv_reason_predefined.  " Vordefinierte Grund

    " Append the text without overwriting existing comments
    IF lo_bo_item->dataref->text IS INITIAL.
      lo_bo_item->dataref->text = iv_reason_text. " Gegebene Grund
    ELSE.
      lo_bo_item->dataref->text = lo_bo_item->dataref->text && ` ` && iv_reason_text.
    ENDIF.

    " 4. Perform the Negative Confirmation (Rückmeldung)
    TRY.
        " Standard Item BO methods to check and execute the confirmation
        lo_bo_item->check_book_confirm_neg( ). "Negative Rückmeldung
        lo_bo_item->book_confirm_neg( ).       "Negative Rückmeldung

        " 5. Save using the inherited SAVE_BO and unlock
        save_bo( lo_bo_item ).
        lo_bo_item->unlock_for_edit( ).

      CATCH cx_eewa_base INTO DATA(lex).
        " Ensure we unlock before passing the error up to Fiori
        lo_bo_item->unlock_for_edit( ).
        RAISE EXCEPTION lex.
    ENDTRY.

  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_WASTE_ORDER_API=>CANCEL_WASTE_ORDER_ITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_POBJNR                      TYPE        CHAR30
* | [--->] IV_REASON_PREDEFINED           TYPE        CHAR255
* | [--->] IV_REASON_TEXT                 TYPE        STRING
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD cancel_waste_order_item.
    " 1. Instantiate this class to access the inherited instance methods
    DATA(lo_api) = NEW zcl_wr_waste_order_api( ).
    " 2. Call the instance method
    lo_api->execute_cancel(
      iv_pobjnr      = iv_pobjnr
      IV_REASON_PREDEFINED = iv_reason_predefined
      iv_reason_text = iv_reason_text
    ).
  ENDMETHOD.
ENDCLASS.
