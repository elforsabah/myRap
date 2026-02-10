CLASS ZCL_WR_TOUR_EXTEND_CALC IMPLEMENTATION.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
    " !!! IMPORTANT: Replace 'CapacityHours' with the EXACT case-sensitive name from your CDS View !!!
    INSERT 'CapacityHours' INTO TABLE et_requested_orig_elements. 
    INSERT 'TourDuration'  INTO TABLE et_requested_orig_elements.
  ENDMETHOD.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
    DATA: lv_capacity_hours TYPE p LENGTH 10 DECIMALS 2,
          lv_total_dur_min  TYPE p LENGTH 10 DECIMALS 2,
          lv_used_min       TYPE p LENGTH 10 DECIMALS 2,
          lv_remaining_min  TYPE p LENGTH 10 DECIMALS 2.

    FIELD-SYMBOLS: <ls_original>   TYPE any,
                   <ls_calculated> TYPE any,
                   <lv_val_hours>  TYPE any,
                   <lv_val_dur>    TYPE any,
                   <lv_set_cap>    TYPE any,
                   <lv_set_rem>    TYPE any.

    LOOP AT it_original_data ASSIGNING <ls_original>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calculated> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " 1. READ Source Fields
      " !!! MATCH THIS NAME WITH THE ONE IN GET_CALCULATION_INFO !!!
      ASSIGN COMPONENT 'CapacityHours' OF STRUCTURE <ls_original> TO <lv_val_hours>.
      ASSIGN COMPONENT 'TourDuration'  OF STRUCTURE <ls_original> TO <lv_val_dur>.

      IF <lv_val_hours> IS ASSIGNED AND <lv_val_dur> IS ASSIGNED.
        lv_capacity_hours = <lv_val_hours>.
        lv_total_dur_min  = <lv_val_dur>.
      ELSE.
        CONTINUE.
      ENDIF.

      " 2. CONVERSION & CALCULATION
      " Convert Hours -> Minutes (e.g. 3.83 hours * 60 = 230 minutes)
      lv_used_min = lv_capacity_hours * 60.

      " Calculate Remaining (Total Minutes - Used Minutes)
      IF lv_total_dur_min > 0.
         lv_remaining_min = lv_total_dur_min - lv_used_min.
      ELSE.
         lv_remaining_min = 0.
      ENDIF.

      " 3. ASSIGN to Virtual Fields
      
      " Set tour_capacity_new (Display as 230)
      ASSIGN COMPONENT 'tour_capacity_new' OF STRUCTURE <ls_calculated> TO <lv_set_cap>.
      IF sy-subrc = 0.
        <lv_set_cap> = lv_used_min.
      ENDIF.

      " Set zz_remaining_duration (Display as 280)
      ASSIGN COMPONENT 'zz_remaining_duration' OF STRUCTURE <ls_calculated> TO <lv_set_rem>.
      IF sy-subrc = 0.
        <lv_set_rem> = lv_remaining_min.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.
ENDCLASS.
