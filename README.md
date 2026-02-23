METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " Request the real numeric field so we can format it
    APPEND 'ZZ_TIMEADJUSTMENT' TO et_requested_orig_elements.
    " (Append any other fields you need for service_criticality, etc.)
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.
    DATA: lv_time_num TYPE i,
          lv_time_str TYPE string.

    FIELD-SYMBOLS: <ls_orig>        TYPE any,
                   <ls_calc>        TYPE any,
                   <lv_val_num>     TYPE any,
                   <lv_set_display> TYPE any.

    LOOP AT it_original_data ASSIGNING <ls_orig>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calc> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " ... (Your existing logic for service_criticality) ...

      " 1. Read the pure number from the database
      ASSIGN COMPONENT 'ZZ_TIMEADJUSTMENT' OF STRUCTURE <ls_orig> TO <lv_val_num>.
      IF sy-subrc = 0 AND <lv_val_num> IS NOT INITIAL.
        lv_time_num = <lv_val_num>.
      ELSE.
        lv_time_num = 0.
      ENDIF.

      " 2. Format the string for the UI (+37 MIN, -15 MIN, etc.)
      IF lv_time_num > 0.
        " Add the plus sign for positive numbers
        lv_time_str = |+{ lv_time_num } MIN|.
      ELSEIF lv_time_num < 0.
        " Minus sign is automatically handled by the number, just add MIN
        lv_time_str = |{ lv_time_num } MIN|.
      ELSE.
        lv_time_str = |0 MIN|.
      ENDIF.

      " 3. Map the formatted string to the Virtual Field for the UI
      ASSIGN COMPONENT 'ZZ_TIMEADJUST_DISPLAY' OF STRUCTURE <ls_calc> TO <lv_set_display>.
      IF sy-subrc = 0.
        <lv_set_display> = lv_time_str.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.
