" --- D. THE MATH ---
      " ... (Your existing math logic) ...

      " --- NEW: Determine Color (Criticality) ---
      DATA lv_crit TYPE int1.
      IF lv_max_dur_min = 0.
        lv_crit = 0. " Neutral/Grey
      ELSEIF lv_new_dur_min > lv_max_dur_min.
        lv_crit = 1. " 1 = RED (Over capacity)
      ELSEIF lv_new_dur_min >= ( lv_max_dur_min * '0.90' ).
        lv_crit = 2. " 2 = YELLOW (Warning: 90% or more full)
      ELSE.
        lv_crit = 3. " 3 = GREEN (Good)
      ENDIF.

      " --- E. Map to UI Virtual Fields ---
      " ... (Your existing mapping) ...

      " Map the color to the UI
      ASSIGN COMPONENT 'ZZ_CAPACITY_CRITICALITY' OF STRUCTURE <ls_calc> TO FIELD-SYMBOL(<lv_set_crit>).
      IF sy-subrc = 0. <lv_set_crit> = lv_crit. ENDIF.
