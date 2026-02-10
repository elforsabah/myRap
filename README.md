class ZCL_WR_TOUR_EXTEND_CALC definition
  public
  create public .

public section.

  interfaces IF_SADL_EXIT .
  interfaces IF_SADL_EXIT_CALC_ELEMENT_READ .
protected section.
private section.
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
METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
  FIELD-SYMBOLS: <ls_original>            TYPE any,
                 <ls_calculated>          TYPE any,
                 <lv_refid>               TYPE any,
                 <lv_service_criticality> TYPE int1,
                 <lv_reactiontime>        TYPE zwr_d_ewmd_remaining_duration.

  CLEAR ct_calculated_data.
*  LOOP AT it_original_data ASSIGNING <ls_original>.
*    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
*    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.
*
*    ASSIGN COMPONENT 'REFERENCEID' OF STRUCTURE <ls_original> TO <lv_refid>.
*    IF sy-subrc = 0.
*      DATA lv_reactiontime TYPE zwr_d_ewmd_reactiontime.
**
**      " Fetch via compliant CDS (not direct table)
*      SELECT SINGLE reaction_time  " aliased field for ZZPOINT_ORIGIN_WDOI
*        FROM zi_wr_ewa_order_object
*        WHERE referenceid = @<lv_refid>
*        INTO @lv_reactiontime.
*      IF sy-subrc = 0.
*        " Map to 1 (red/critical if replanned) or 0 (neutral) — fixed reversed logic
*        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_service_criticality>.
*        IF sy-subrc = 0.
*          <lv_service_criticality> = COND #( WHEN lv_reactiontime IS NOT INITIAL THEN 1 ELSE 0 ).
*        ENDIF.
*      ENDIF.
*    ENDIF.
*  ENDLOOP.
ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_TOUR_EXTEND_CALC->IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_REQUESTED_CALC_ELEMENTS     TYPE        TT_ELEMENTS
* | [--->] IV_ENTITY                      TYPE        STRING
* | [<---] ET_REQUESTED_ORIG_ELEMENTS     TYPE        TT_ELEMENTS
* | [!CX!] CX_SADL_EXIT
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
   " Request fields needed for calculations (case-sensitive CDS names)
*    APPEND 'REFERENCEID' TO et_requested_orig_elements.

  endmethod.
ENDCLASS.
