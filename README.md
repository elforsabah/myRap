METHOD if_sadl_exit_calc_element_read~calculate.
  FIELD-SYMBOLS: <ls_original> TYPE any,
                 <ls_calculated> TYPE any,
                 <lv_ServiceUUID> TYPE sysuuid_x16,
                 <lv_point_of_origin> TYPE abap_int1.

  CLEAR ct_calculated_data.

  LOOP AT it_original_data ASSIGNING <ls_original>.
    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

    ASSIGN COMPONENT 'SERVICEUUID' OF STRUCTURE <ls_original> TO <lv_ServiceUUID>.
    IF sy-subrc = 0.
      DATA(lv_zzpoint_origin_wdoi) = VALUE abap_char1( ).

      " Fetch via compliant CDS (not direct table)
      SELECT SINGLE point_of_origin  " aliased field for ZZPOINT_ORIGIN_WDOI
        FROM zi_wr_ewa_order_object
        WHERE PdServiceUuid = @<lv_ServiceUUID>
        INTO @lv_zzpoint_origin_wdoi.

      IF sy-subrc = 0.
        " Map to 1 (red/critical if replanned) or 0 (neutral) — fixed reversed logic
        ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin>.
        IF sy-subrc = 0.
          <lv_point_of_origin> = COND #( WHEN lv_zzpoint_origin_wdoi = 'X' THEN 1 ELSE 0 ).
        ENDIF.
      ELSE.
        " Fallback: Neutral if no data
        IF <lv_point_of_origin> IS ASSIGNED.
          <lv_point_of_origin> = 0.
        ENDIF.
      ENDIF.
    ENDIF.
  ENDLOOP.
ENDMETHOD.
