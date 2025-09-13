CLASS zcl_wr_service_extend_calc DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_sadl_exit_calc_element_read.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_service_extend_calc IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " Request fields needed for calculations (case-sensitive CDS names)
    APPEND 'SERVICEUUID' TO et_requested_orig_elements.
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.
    FIELD-SYMBOLS: <ls_original> TYPE any,
                   <ls_calculated> TYPE any,
                   <lv_ServiceUUID> TYPE sysuuid_x16,  // Adjust if UUID type differs
                   <lv_point_of_origin> TYPE abap_char1.

    CLEAR ct_calculated_data.

    LOOP AT it_original_data ASSIGNING <ls_original>.
      APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
      MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.

      ASSIGN COMPONENT 'SERVICEUUID' OF STRUCTURE <ls_original> TO <lv_ServiceUUID>.
      IF sy-subrc = 0.
        DATA(lv_zzpoint_origin_wdoi) = VALUE abap_char1( ).  // Temp for fetched value

        " Fetch via compliant CDS (not direct table)
        SELECT SINGLE point_of_origin  // Your aliased field for ZZPOINT_ORIGIN_WDOI
          FROM zi_wr_ewa_order_object
          WHERE pd_service_uuid = @<lv_ServiceUUID>
          INTO @lv_zzpoint_origin_wdoi.

        IF sy-subrc = 0.
          " Map to '1' (red/critical if replanned) or '0' (neutral)
          ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin>.
          IF sy-subrc = 0.
            <lv_point_of_origin> = COND #( WHEN lv_zzpoint_origin_wdoi = 'X' THEN '1' ELSE '0' ).
          ENDIF.
        ELSE.
          " Fallback: Neutral if no data
          IF <lv_point_of_origin> IS ASSIGNED.
            <lv_point_of_origin> = '0'.
          ENDIF.
        ENDIF.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
