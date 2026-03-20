 METHOD terminateservice.
    DATA: lt_unassign_keys  TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour\\serviceassignment~unassign,
          lt_service_update TYPE TABLE FOR UPDATE /plce/r_pdservice,
          lt_history_create TYPE TABLE FOR CREATE /plce/r_pdservice\_statushistory,
          lt_return         TYPE bapiret2_t.

    " 1. Read the service to get POBJNR (ReferenceInternalId)
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      FIELDS ( serviceuuid referenceinternalid ReferenceId servicestatus serviceid )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " 2. Process the Storno for each selected service
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.
      CLEAR lt_return.

      " -------------------------------------------------------------------------
      " Call the RFC to bypass the RAP strict mode dump
      " -------------------------------------------------------------------------
      CALL FUNCTION 'Z_WR_CANCEL_WASTE_ORDER_RFC' DESTINATION 'NONE'
        EXPORTING
          iv_pobjnr            = CONV char30( <ls_service>-referenceinternalid )
          iv_reason_predefined = CONV char8( ls_param-definiertegrund )
          iv_reason_text       = CONV string( ls_param-stornogrund )
        IMPORTING
          et_return            = lt_return.

      " 3. Evaluate the RFC result
      IF lt_return IS INITIAL.
        " --- SUCCESS ---

        " A. Prepare Status Updates (Status 01)
        APPEND VALUE #( %tky = <ls_service>-%tky
                        servicestatus = '01' ) TO lt_service_update.

        APPEND VALUE #( %tky = <ls_service>-%tky
                        %target = VALUE #( ( %cid = |HIST_{ sy-tabix }|
                                             servicestatus = '01' ) ) ) TO lt_history_create.

        " B. Prepare Tour Unassignment (Only unassign if Storno succeeds!)
        SELECT SINGLE asgmt~serviceuuid, asgmt~touruuid
          FROM /plce/r_pdtourserviceasgmt AS asgmt
          WHERE asgmt~serviceuuid = @<ls_service>-serviceuuid
          INTO @DATA(ls_unassign).

        IF sy-subrc = 0.

          APPEND VALUE #( %key-touruuid    = ls_unassign-touruuid
                          %key-serviceuuid = ls_unassign-serviceuuid ) TO lt_unassign_keys.
        ENDIF.


        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT'
                                            number   = '009'
                                            severity = if_abap_behv_message=>severity-information
                                            v1       = <ls_service>-ReferenceId  ) "leading zeros will be removed
                      ) TO reported-service.


      ELSE.
        " --- FAILURE ---
        " The backend rejected the Storno (e.g. the WA19 error).
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

    " 4. Bulk Modify RAP Entities (Status Updates)
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
        ENTITY service
          UPDATE FIELDS ( servicestatus )
          WITH lt_service_update
          CREATE BY \_statushistory
          SET FIELDS WITH lt_history_create.
    ENDIF.

    " 5. Bulk Execute Tour Unassignment (Only for successful Stornos)
    IF lt_unassign_keys IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdtour
        ENTITY serviceassignment
          EXECUTE unassign FROM lt_unassign_keys
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).

      /plce/cl_base_misc=>check_response(
        EXPORTING is_response = srvc_unassign_reported
        CHANGING  ct_messages = reported-%other ).
    ENDIF.

    " 6. Return the updated data to the UI
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.



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
                   <lv_time_disp>           TYPE any.

    DATA: lv_reactiontime TYPE zwr_d_ewmd_reactiontime,
          lv_time_num     TYPE i,
          lv_time_str     TYPE string.

    CLEAR ct_calculated_data.

    LOOP AT it_original_data ASSIGNING <ls_original>.
      " Prepare the calculated row
      APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
      MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

      " =======================================================================
      " LOGIC 1: Service Criticality
      " =======================================================================
      ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
      IF sy-subrc = 0 AND <lv_refid> IS NOT INITIAL.

        CLEAR lv_reactiontime.
        " Fetch via compliant CDS
        SELECT SINGLE reaction_time  " aliased field for ZZPOINT_ORIGIN_WDOI
          FROM zi_wr_ewa_order_object
          WHERE referenceid = @<lv_refid>
          INTO @lv_reactiontime.

        IF sy-subrc = 0.
          ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
          IF sy-subrc = 0.
            " Map to 1 (red/critical if replanned) or 0 (neutral)
            <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
          ENDIF.
        ENDIF.
      ENDIF.

      " =======================================================================
      " LOGIC 2: Time Adjustment Formatting (+37 MIN)
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

    ENDLOOP.
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " Request fields needed for calculations (Case-sensitive standard CDS names)
    " Field needed for LOGIC 1 (Criticality)
    APPEND 'REFERENCEID' TO et_requested_orig_elements.

    " Field needed for LOGIC 2 (Time Adjustment Display)
    APPEND 'ZZ_TIMEADJUSTMENT' TO et_requested_orig_elements.

  ENDMETHOD.
ENDCLASS.

