METHOD if_sadl_exit_calc_element_read~calculate.
  FIELD-SYMBOLS: <ls_original> TYPE any,
                 <ls_calculated> TYPE any,
                 <lv_point_of_origin> TYPE any.  " Adjust type if known, e.g., abap.int1

  CLEAR ct_calculated_data.

  LOOP AT it_original_data ASSIGNING <ls_original>.
    APPEND INITIAL LINE TO ct_calculated_data ASSIGNING <ls_calculated>.
    MOVE-CORRESPONDING <ls_original> TO <ls_calculated>.
    " Set the virtual element
    ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO <lv_point_of_origin>.
    IF sy-subrc = 0.
    ASSIGN COMPONENT 'SERVICEUUID' OF STRUCTURE <ls_original> TO FIELD-SYMBOL(<lv_ServiceUUID>).

      select SINGLE * from EWA_ORDER_OBJECT WHERE /PLCP/PD_SERVICE_UUID = @<lv_ServiceUUID>  into  @data(ls_ORDER_OBJECT).

      <lv_point_of_origin> = ls_ORDER_OBJECT-ZZPOINT_ORIGIN_WDOI.  " Replace with your actual logic

      
    ENDIF.
  ENDLOOP.
ENDMETHOD.

