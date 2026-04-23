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
         FIELDS ( referenceid serviceuuid )
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


    " Declare bulk operation tables (same pattern as terminateservice)
    DATA: lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
          lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom.

    " Read ALL services at once (same pattern as terminateservice)
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid referenceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " Loop through SERVICES (not keys!) - same pattern as terminateservice
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

      " Get the parameter for THIS service
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

      " Check if ExtCustom exists for this service
      READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        FIELDS ( serviceuuid )
        WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
        " ExtCustom doesn't exist - prepare for creation
        DATA(lv_cid) = |$cid_{ <ls_service>-serviceuuid }$|.

        APPEND VALUE #( serviceuuid = <ls_service>-serviceuuid
                        %target = VALUE #( ( %cid = lv_cid ) ) )
          TO lt_ext_create.
      ENDIF.

      " Prepare the update (will be applied after creation if needed)
      APPEND VALUE #( %key-serviceuuid = <ls_service>-serviceuuid
                      WDPLANTNR = ls_param-anlage )
        TO lt_ext_update.

      CLEAR lt_extcustom.

    ENDLOOP.

    " Bulk create missing ExtCustom records (same pattern as terminateservice)
    IF lt_ext_create IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
        CREATE BY \_extcustom
        AUTO FILL CID SET FIELDS WITH lt_ext_create
        MAPPED DATA(lmapped)
        FAILED DATA(lfailed)
        REPORTED DATA(lreported).

      IF lfailed IS NOT INITIAL.
        APPEND LINES OF lfailed-extcustom TO failed-extcustom.
        APPEND LINES OF lreported-extcustom TO reported-extcustom.
      ENDIF.
    ENDIF.

    " Bulk update disposal facility (same pattern as terminateservice)
    IF lt_ext_update IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY extcustom
        UPDATE FIELDS ( WDPLANTNR )
        WITH lt_ext_update
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).

      IF lt_failed_update IS NOT INITIAL.
        APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
        APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.
      ELSE.
        " Success - add messages for each service
        LOOP AT lt_services ASSIGNING <ls_service>.
          DATA(ls_param_msg) = keys[ %tky = <ls_service>-%tky ]-%param.

          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '015'
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = <ls_service>-ReferenceId )
                        ) TO reported-service.
        ENDLOOP.
      ENDIF.
    ENDIF.

    " Return updated data to UI
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.

  METHOD precheck_anlageaendern.

   " Read ALL services at once
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid referenceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " Loop through SERVICES (not keys!)
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

      " Get the parameter for THIS service
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

      " Get order details
      SELECT SINGLE *
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @<ls_service>-referenceid
        INTO @DATA(ls_order).

      IF sy-subrc <> 0.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 1: Check for Fixed Disposal Contract
      " ---------------------------------------------------------------------
      IF ls_order-entsorgunganlage IS NOT INITIAL.
        SELECT SINGLE ZZ_FRAMEWORK_AGREEMENT
          FROM ewa_el_wdplant
          WHERE wdplantnr = @ls_order-entsorgunganlage
          INTO @DATA(lv_existing_contract).

        IF sy-subrc = 0 AND lv_existing_contract IS NOT INITIAL.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '010'
                                   severity = if_abap_behv_message=>severity-error
                                   )
                        ) TO reported-service.
          CONTINUE.
        ENDIF.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 2: Check if facility is selected
      " ---------------------------------------------------------------------
      IF ls_param-anlage IS INITIAL.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '011'
                                 severity = if_abap_behv_message=>severity-error
                                  )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 3: Check facility exists and is available
      " ---------------------------------------------------------------------
      SELECT SINGLE wdplantnr, zz_available_dispo, ZZ_FRAMEWORK_AGREEMENT
        FROM ewa_el_wdplant
        WHERE wdplantnr = @ls_param-anlage
        INTO @DATA(ls_plant).

      IF sy-subrc <> 0.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '012'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = CONV #( ls_param-anlage ) )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      IF ls_plant-zz_available_dispo IS INITIAL OR ls_plant-zz_available_dispo <> 'X'.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message(
                                 id       = 'Z_MSG_SVR_TOUR_EXT'
                                 number   = '013'
                                 severity = if_abap_behv_message=>severity-error
                                 v1       = CONV #( ls_param-anlage ) )
                      ) TO reported-service.
        CONTINUE.
      ENDIF.

      " ---------------------------------------------------------------------
      " Validation 4: AVV-Code matching
      " ---------------------------------------------------------------------
      SELECT COUNT(*)
        FROM /watp/twdplanavv
        WHERE wdplantnr = @ls_param-anlage
        INTO @DATA(lv_avv_count).

      IF lv_avv_count > 0.
        " AVV assignments exist - must validate
        SELECT SINGLE /WATP/AVVCODE
          FROM ewa_order_object
          WHERE pobjnr = @<ls_service>-referenceinternalid
          INTO @DATA(lv_order_avvcode).

        IF sy-subrc = 0 AND lv_order_avvcode IS NOT INITIAL.
          SELECT SINGLE avvcode
            FROM /watp/twdplanavv
            WHERE wdplantnr = @ls_param-anlage
              AND avvcode = @lv_order_avvcode
            INTO @DATA(lv_found_avv).

          IF sy-subrc <> 0.
            APPEND VALUE #( %tky = <ls_service>-%tky
                            %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-service.
            APPEND VALUE #( %tky = <ls_service>-%tky
                            %msg = new_message(
                                     id       = 'Z_MSG_SVR_TOUR_EXT'
                                     number   = '014'
                                     severity = if_abap_behv_message=>severity-error
                                     v1       = CONV #( lv_order_avvcode ) )
                          ) TO reported-service.
            CONTINUE.
          ENDIF.
        ENDIF.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
