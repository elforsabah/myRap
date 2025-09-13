METHOD if_sadl_exit_calc_element_read~calculate.
  ct_calculated_data = it_original_data.  " Copy input to output as base

  LOOP AT ct_calculated_data ASSIGNING FIELD-SYMBOL(<ls_calculated>).
    " Assign the virtual element component dynamically
    ASSIGN COMPONENT 'POINT_OF_ORIGIN' OF STRUCTURE <ls_calculated> TO FIELD-SYMBOL(<lv_point_of_origin>).
    IF sy-subrc = 0.
      " Add your custom logic here
      " Example: Simple static value
      <lv_point_of_origin> = 1.

      " Or based on other fields (dynamically access them if needed)
      " ASSIGN COMPONENT 'SERVICE_TYPE' OF STRUCTURE <ls_calculated> TO FIELD-SYMBOL(<lv_service_type>).
      " IF sy-subrc = 0 AND <lv_service_type> = 'ABC'.
      "   <lv_point_of_origin> = 1.
      " ELSE.
      "   <lv_point_of_origin> = 0.
      " ENDIF.
    ENDIF.
  ENDLOOP.
ENDMETHOD.
