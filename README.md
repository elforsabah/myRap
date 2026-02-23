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
