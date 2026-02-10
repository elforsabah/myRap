CLASS ZCL_WR_TOUR_EXTEND_CALC IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    APPEND 'TourCapacity' TO et_requested_orig_elements. 
    APPEND 'TourDuration' TO et_requested_orig_elements.
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.
    DATA: lv_tour_dur_hours   TYPE p LENGTH 10 DECIMALS 2,
          lv_capacity_percent TYPE p LENGTH 10 DECIMALS 2,
          
          lv_total_min        TYPE p LENGTH 10 DECIMALS 0,
          lv_used_min         TYPE p LENGTH 10 DECIMALS 0,
          lv_remaining_min    TYPE p LENGTH 10 DECIMALS 0.

    FIELD-SYMBOLS: <ls_orig>    TYPE any,
                   <ls_calc>    TYPE any,
                   <lv_val_cap> TYPE any,
                   <lv_val_dur> TYPE any,
                   <lv_set_cap> TYPE any,
                   <lv_set_rem> TYPE any,
                   <lv_set_tot> TYPE any.

    LOOP AT it_original_data ASSIGNING <ls_orig>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calc> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " 1. Get Source Values
      ASSIGN COMPONENT 'TourCapacity' OF STRUCTURE <ls_orig> TO <lv_val_cap>.
      ASSIGN COMPONENT 'TourDuration' OF STRUCTURE <ls_orig> TO <lv_val_dur>.

      IF <lv_val_cap> IS ASSIGNED AND <lv_val_dur> IS ASSIGNED.
         lv_capacity_percent = <lv_val_cap>. " e.g., 62.35
         lv_tour_dur_hours   = <lv_val_dur>. " e.g., 5.30
      ELSE.
         CONTINUE.
      ENDIF.

      " 2. Calculate Everything in MINUTES
      
      " A. Total Duration: 5.3 Hours * 60 = 318 Minutes
      lv_total_min = lv_tour_dur_hours * 60.

      " B. Used Duration: 318 Minutes * 62.35% = 198 Minutes
      IF lv_total_min > 0.
        lv_used_min = ( lv_total_min * lv_capacity_percent ) / 100.
      ELSE.
        lv_used_min = 0.
      ENDIF.

      " C. Remaining: 318 - 198 = 120 Minutes
      lv_remaining_min = lv_total_min - lv_used_min.

      " 3. Write outputs
      
      " Output 1: Used Capacity
      ASSIGN COMPONENT 'TOUR_CAPACITY_NEW' OF STRUCTURE <ls_calc> TO <lv_set_cap>.
      IF sy-subrc = 0. <lv_set_cap> = lv_used_min. ENDIF.

      " Output 2: Remaining
      ASSIGN COMPONENT 'ZZ_REMAINING_DURATION' OF STRUCTURE <ls_calc> TO <lv_set_rem>.
      IF sy-subrc = 0. <lv_set_rem> = lv_remaining_min. ENDIF.

      " Output 3: Total (Target for Progress Bar)
      ASSIGN COMPONENT 'TOUR_DURATION_MIN' OF STRUCTURE <ls_calc> TO <lv_set_tot>.
      IF sy-subrc = 0. <lv_set_tot> = lv_total_min. ENDIF.

    ENDLOOP.
  ENDMETHOD.
ENDCLASS.
