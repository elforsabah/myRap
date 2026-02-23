class ZCL_WR_SERVICE_EXTEND_CALC definition
  public
  create public .

public section.

  interfaces IF_SADL_EXIT .
  interfaces IF_SADL_EXIT_CALC_ELEMENT_READ .
protected section.
private section.
ENDCLASS.



CLASS ZCL_WR_SERVICE_EXTEND_CALC IMPLEMENTATION.


METHOD if_sadl_exit_calc_element_read~calculate.
  FIELD-SYMBOLS: <ls_original>            TYPE any,
                 <ls_calculated>          TYPE any,
                 <lv_refid>               TYPE any,
                 <lv_service_criticality> TYPE int1,
                 <lv_reactiontime>        TYPE zwr_d_ewmd_reactiontime.

  CLEAR ct_calculated_data.
  DATA lv_test TYPE string.
  LOOP AT it_original_data ASSIGNING <ls_original>.
    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

    ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
    IF sy-subrc = 0.
      DATA lv_reactiontime TYPE zwr_d_ewmd_reactiontime.
*
*      " Fetch via compliant CDS (not direct table)
      SELECT SINGLE reaction_time  " aliased field for ZZPOINT_ORIGIN_WDOI
        FROM zi_wr_ewa_order_object
        WHERE referenceid = @<lv_refid>
        INTO @lv_reactiontime.
      IF sy-subrc = 0.
        " Map to 1 (red/critical if replanned) or 0 (neutral) — fixed reversed logic
        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
        IF sy-subrc = 0.
          <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
        ENDIF.
      ENDIF.
    ENDIF.
  ENDLOOP.
  
ENDMETHOD.


  method IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
   " Request fields needed for calculations (case-sensitive CDS names)
    APPEND 'REFERENCEID' TO et_requested_orig_elements.

  endmethod.
ENDCLASS.
