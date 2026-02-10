CLASS ZCL_WR_TOUR_EXTEND_CALC IMPLEMENTATION.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~GET_CALCULATION_INFO.
    " 1. Request the source fields needed for calculation
    "    Note: Field names must match the CDS view Element names exactly (Case Sensitive)
    INSERT 'TourCapacity' INTO TABLE et_requested_orig_elements.
    INSERT 'TourDuration' INTO TABLE et_requested_orig_elements.
  ENDMETHOD.

  METHOD IF_SADL_EXIT_CALC_ELEMENT_READ~CALCULATE.
    " Define local variables for cleaner calculation
    DATA: lv_tour_capacity TYPE p LENGTH 5 DECIMALS 2, " Adjust type if needed
          lv_tour_duration TYPE p LENGTH 10 DECIMALS 2,
          lv_used_duration TYPE p LENGTH 10 DECIMALS 2,
          lv_remaining     TYPE p LENGTH 10 DECIMALS 2.

    FIELD-SYMBOLS: <ls_original>   TYPE any,
                   <ls_calculated> TYPE any,
                   <lv_val_cap>    TYPE any,
                   <lv_val_dur>    TYPE any,
                   <lv_set_cap>    TYPE any,
                   <lv_set_rem>    TYPE any.

    " 2. Loop through the data to perform calculations
    LOOP AT it_original_data ASSIGNING <ls_original>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calculated> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " --- A. Get Source Values ---
      ASSIGN COMPONENT 'TourCapacity' OF STRUCTURE <ls_original> TO <lv_val_cap>.
      ASSIGN COMPONENT 'TourDuration' OF STRUCTURE <ls_original> TO <lv_val_dur>.

      IF <lv_val_cap> IS ASSIGNED AND <lv_val_dur> IS ASSIGNED.
        lv_tour_capacity = <lv_val_cap>.
        lv_tour_duration = <lv_val_dur>.
      ELSE.
        CONTINUE. 
      ENDIF.

      " --- B. Requirement 1: Copy TourCapacity to tour_capacity_new ---
      ASSIGN COMPONENT 'tour_capacity_new' OF STRUCTURE <ls_calculated> TO <lv_set_cap>.
      IF sy-subrc = 0.
        <lv_set_cap> = lv_tour_capacity.
      ENDIF.

      " --- C. Requirement 3: Calculate Remaining Duration ---
      " Logic: Remaining = Duration - (Duration * Capacity% / 100)
      ASSIGN COMPONENT 'zz_remaining_duration' OF STRUCTURE <ls_calculated> TO <lv_set_rem>.
      IF sy-subrc = 0.
        IF lv_tour_duration IS NOT INITIAL.
             " Calculate Used Time first
             lv_used_duration = ( lv_tour_duration * lv_tour_capacity ) / 100.
             " Calculate Remaining
             lv_remaining = lv_tour_duration - lv_used_duration.
             
             <lv_set_rem> = lv_remaining.
        ELSE.
             <lv_set_rem> = 0.
        ENDIF.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.

ENDCLASS.
