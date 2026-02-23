METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " Request the base Tour fields needed for math and matching
    INSERT 'DRIVINGTIME'             INTO TABLE et_requested_orig_elements.
    INSERT 'TOURCAPACITY'            INTO TABLE et_requested_orig_elements.
    INSERT 'TOURDURATION'            INTO TABLE et_requested_orig_elements.
    INSERT 'TOURUUID'                INTO TABLE et_requested_orig_elements.
    
    " --- NEW: Request the MaximumTourDuration and its Unit ---
    INSERT 'MAXIMUMTOURDURATION'     INTO TABLE et_requested_orig_elements.
    INSERT 'MAXIMUMTOURDURATIONUNIT' INTO TABLE et_requested_orig_elements.
  ENDMETHOD.
