CLASS zcl_wr_service_extend_calc DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_sadl_exit .
    INTERFACES if_sadl_exit_calc_element_read .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.



CLASS ZCL_WR_SERVICE_EXTEND_CALC IMPLEMENTATION.


  METHOD if_sadl_exit_calc_element_read~calculate.
    FIELD-SYMBOLS: <ls_original>            TYPE any,
                   <ls_calculated>          TYPE any,
                   <lv_refid>               TYPE any,
                   <lv_service_criticality> TYPE int1,
                   <lv_time_adj>            TYPE any,
                   <lv_entsorgungs>            TYPE any,
                   <lv_time_disp>           TYPE any,
                   <lv_main_position>       TYPE any,
                   <lv_main_pos_crit>       TYPE int1.

    DATA: lv_reactiontime TYPE zwr_d_ewmd_reactiontime,
          lv_time_num     TYPE i,
          lv_time_str     TYPE string.

    CLEAR ct_calculated_data.

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
          WHERE referenceid = @<lv_refid> into @data(ls_wr_ewa_order_object).
         lv_reactiontime  = ls_wr_ewa_order_object-reaction_time.

         data(lv_entsorgungsanlage) = ls_wr_ewa_order_object-entsorgunganlage.

        IF sy-subrc = 0.
          ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
          IF sy-subrc = 0.
            " Map to 1 (red/critical if replanned) or 0 (neutral)
            <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
          ENDIF.

         ASSIGN COMPONENT 'ZZ_ENTSORGUNGANLAGE' OF STRUCTURE <ls_calculated> TO <lv_entsorgungs>.
         IF sy-subrc = 0.
        <lv_entsorgungs> = ls_wr_ewa_order_object-entsorgunganlage.
        endif.
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


      " =======================================================================
      "  Main Position Criticality (Green for Grouped Services)
      " =======================================================================
      ASSIGN COMPONENT 'ZZ_MAIN_POSITION' OF STRUCTURE <ls_original> TO <lv_main_position>.
      IF sy-subrc = 0.
        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_main_pos_crit>.
        IF sy-subrc = 0.
          " Check if main position is filled

*           <lv_main_position> = 2.
          IF <lv_main_position> IS NOT INITIAL.
            " Service belongs to a group -> Green (Success)
            <lv_main_pos_crit> = 3.
          ELSE.
            " Service does not belong to a group -> Neutral
            <lv_main_pos_crit> = 0.
          ENDIF.
        ENDIF.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.


  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    APPEND 'REFERENCEID' TO et_requested_orig_elements.
    APPEND 'ZZ_TIMEADJUSTMENT' TO et_requested_orig_elements.

  ENDMETHOD.
ENDCLASS.
