METHOD if_sadl_exit_calc_element_read~calculate.
    " ... (Previous variable definitions) ...
    FIELD-SYMBOLS: <lv_set_unit> TYPE any. " <--- Add this

    LOOP AT it_original_data ASSIGNING <ls_orig>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calc> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " ... (Your existing calculation logic for minutes) ...
      
      " --- EXISTING ASSIGNMENTS ---
      ASSIGN COMPONENT 'TOUR_CAPACITY_NEW' OF STRUCTURE <ls_calc> TO <lv_set_cap>.
      IF sy-subrc = 0. <lv_set_cap> = lv_used_min. ENDIF.

      ASSIGN COMPONENT 'ZZ_REMAINING_DURATION' OF STRUCTURE <ls_calc> TO <lv_set_rem>.
      IF sy-subrc = 0. <lv_set_rem> = lv_remaining_min. ENDIF.
      
      ASSIGN COMPONENT 'TOUR_DURATION_MIN' OF STRUCTURE <ls_calc> TO <lv_set_tot>.
      IF sy-subrc = 0. <lv_set_tot> = lv_total_min. ENDIF.

      " --- NEW: SET THE UNIT TO 'MIN' ---
      ASSIGN COMPONENT 'ZZ_DURATION_UNIT' OF STRUCTURE <ls_calc> TO <lv_set_unit>.
      IF sy-subrc = 0. 
        <lv_set_unit> = 'MIN'. 
      ENDIF.

    ENDLOOP.
ENDMETHOD.
