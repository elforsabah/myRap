CLASS ZCL_WR_TOUR_EXTEND_CALC IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " 1. Request the base Tour fields needed
    APPEND 'TOURUUID'     TO et_requested_orig_elements.
    APPEND 'TOURDURATION' TO et_requested_orig_elements.
    APPEND 'DRIVINGTIME'  TO et_requested_orig_elements.
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.
    DATA: lt_tour_keys    TYPE TABLE OF sysuuid_x16, " Adjust type if your UUID is CHAR32
          lv_tour_uuid    TYPE sysuuid_x16,
          
          lv_tour_dur_h   TYPE p LENGTH 10 DECIMALS 2,
          lv_driving_h    TYPE p LENGTH 10 DECIMALS 2,
          
          lv_tour_dur_min TYPE p LENGTH 10 DECIMALS 0,
          lv_driving_min  TYPE p LENGTH 10 DECIMALS 0,
          
          lv_svc_dur_h    TYPE p LENGTH 10 DECIMALS 2,
          lv_svc_dur_min  TYPE p LENGTH 10 DECIMALS 0,
          lv_svc_adj_min  TYPE p LENGTH 10 DECIMALS 0,
          
          lv_new_dur_min  TYPE p LENGTH 10 DECIMALS 0,
          lv_rem_min      TYPE p LENGTH 10 DECIMALS 0.

    FIELD-SYMBOLS: <ls_orig>      TYPE any,
                   <ls_calc>      TYPE any,
                   <lv_val_uuid>  TYPE any,
                   <lv_val_dur>   TYPE any,
                   <lv_val_drive> TYPE any,
                   
                   <lv_set_cap>   TYPE any,
                   <lv_set_rem>   TYPE any,
                   <lv_set_tot>   TYPE any,
                   <lv_set_unit>  TYPE any.

    " =========================================================================
    " STEP 1: Collect all Tour UUIDs to fetch Service data efficiently
    " =========================================================================
    LOOP AT it_original_data ASSIGNING <ls_orig>.
      ASSIGN COMPONENT 'TOURUUID' OF STRUCTURE <ls_orig> TO <lv_val_uuid>.
      IF sy-subrc = 0 AND <lv_val_uuid> IS NOT INITIAL.
        APPEND <lv_val_uuid> TO lt_tour_keys.
      ENDIF.
    ENDLOOP.

    " =========================================================================
    " STEP 2: Fetch assigned Services (TotalDuration & zz_timeadjustment)
    " =========================================================================
    TYPES: BEGIN OF ty_service_data,
             touruuid         TYPE sysuuid_x16,
             totalduration    TYPE p LENGTH 10 DECIMALS 2,
             zz_timeadjust    TYPE p LENGTH 10 DECIMALS 2,
           END OF ty_service_data.
    DATA: lt_service_raw TYPE TABLE OF ty_service_data.

    IF lt_tour_keys IS NOT INITIAL.
      " Select the raw service data assigned to these tours
      SELECT asg~TourUUID, 
             svc~TotalDuration, 
             svc~zz_timeadjustment AS zz_timeadjust
        FROM /PLCE/C_PDMNLTourServiceAsgWR AS asg
        INNER JOIN /PLCE/C_PDMNLServiceWR AS svc
          ON asg~ServiceUUID = svc~ServiceUUID
        FOR ALL ENTRIES IN @lt_tour_keys
        WHERE asg~TourUUID = @lt_tour_keys-table_line
        INTO TABLE @lt_service_raw.
    ENDIF.

    " =========================================================================
    " STEP 3: Perform calculations row by row
    " =========================================================================
    LOOP AT it_original_data ASSIGNING <ls_orig>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calc> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " --- A. Get Tour Fields ---
      ASSIGN COMPONENT 'TOURUUID'     OF STRUCTURE <ls_orig> TO <lv_val_uuid>.
      ASSIGN COMPONENT 'TOURDURATION' OF STRUCTURE <ls_orig> TO <lv_val_dur>.
      ASSIGN COMPONENT 'DRIVINGTIME'  OF STRUCTURE <ls_orig> TO <lv_val_drive>.

      IF sy-subrc <> 0. CONTINUE. ENDIF.
      
      lv_tour_uuid  = <lv_val_uuid>.
      lv_tour_dur_h = COND #( WHEN <lv_val_dur> IS ASSIGNED THEN <lv_val_dur> ELSE 0 ).
      lv_driving_h  = COND #( WHEN <lv_val_drive> IS ASSIGNED THEN <lv_val_drive> ELSE 0 ).

      " --- B. Convert Tour fields to Minutes ---
      lv_tour_dur_min = lv_tour_dur_h * 60.
      lv_driving_min  = lv_driving_h * 60.

      " --- C. Sum up Service Data for this specific Tour ---
      CLEAR: lv_svc_dur_h, lv_svc_adj_min.
      LOOP AT lt_service_raw INTO DATA(ls_svc) WHERE touruuid = lv_tour_uuid.
        lv_svc_dur_h   = lv_svc_dur_h   + ls_svc-totalduration.
        lv_svc_adj_min = lv_svc_adj_min + ls_svc-zz_timeadjust.
      ENDLOOP.
      
      " Convert Service TotalDuration to Minutes
      lv_svc_dur_min = lv_svc_dur_h * 60.

      " --- D. The Math Requirements ---
      " 1. NewDuration = TotalDuration(Min) + DrivingTime(Min) + zz_timeadjustment(Min)
      lv_new_dur_min = lv_svc_dur_min + lv_driving_min + lv_svc_adj_min.

      " 2. Remaining = TourDuration(Min) - NewDuration
      lv_rem_min = lv_tour_dur_min - lv_new_dur_min.

      " --- E. Map to UI Virtual Fields ---
      ASSIGN COMPONENT 'TOUR_CAPACITY_NEW' OF STRUCTURE <ls_calc> TO <lv_set_cap>.
      IF sy-subrc = 0. <lv_set_cap> = lv_new_dur_min. ENDIF.

      ASSIGN COMPONENT 'TOUR_DURATION_MIN' OF STRUCTURE <ls_calc> TO <lv_set_tot>.
      IF sy-subrc = 0. <lv_set_tot> = lv_tour_dur_min. ENDIF.

      ASSIGN COMPONENT 'ZZ_REMAINING_DURATION' OF STRUCTURE <ls_calc> TO <lv_set_rem>.
      IF sy-subrc = 0. <lv_set_rem> = lv_rem_min. ENDIF.

      ASSIGN COMPONENT 'ZZ_DURATION_UNIT' OF STRUCTURE <ls_calc> TO <lv_set_unit>.
      IF sy-subrc = 0. <lv_set_unit> = 'MIN'. ENDIF.

    ENDLOOP.

  ENDMETHOD.
ENDCLASS.
