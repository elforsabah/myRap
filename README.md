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
      <lv_point_of_origin> = 1.  " Replace with your actual logic

      " If logic depends on other fields, access them dynamically, e.g.:
      " ASSIGN COMPONENT 'SERVICETYPE' OF STRUCTURE <ls_original> TO FIELD-SYMBOL(<lv_service_type>).
      " IF sy-subrc = 0 AND <lv_service_type> = 'ABC'.
      "   <lv_point_of_origin> = 1.
      " ELSE.
      "   <lv_point_of_origin> = 0.
      " ENDIF.
    ENDIF.
  ENDLOOP.
ENDMETHOD.
