CLASS zcl_wr_tour_extend_calc DEFINITION
  PUBLIC
  CREATE PUBLIC .

  PUBLIC SECTION.
    INTERFACES if_sadl_exit .
    INTERFACES if_sadl_exit_calc_element_read .
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_tour_extend_calc IMPLEMENTATION.

  METHOD if_sadl_exit_calc_element_read~get_calculation_info.
    " Request the base Tour fields needed for math and matching
    APPEND 'DRIVINGTIME'             TO et_requested_orig_elements.
    APPEND 'TOURCAPACITY'            TO et_requested_orig_elements.
    APPEND 'TOURDURATION'            TO et_requested_orig_elements.
    APPEND 'TOURUUID'                TO et_requested_orig_elements.
    
    " --- NEW: Request the MaximumTourDuration and its Unit ---
    APPEND 'MAXIMUMTOURDURATION'     TO et_requested_orig_elements.
    APPEND 'MAXIMUMTOURDURATIONUNIT' TO et_requested_orig_elements.
  ENDMETHOD.

  METHOD if_sadl_exit_calc_element_read~calculate.
    " -------------------------------------------------------------------------
    " 1. Data Declarations
    " -------------------------------------------------------------------------
    TYPES: BEGIN OF ty_tour_key,
             touruuid TYPE sysuuid_x16,
           END OF ty_tour_key.

    TYPES: BEGIN OF ty_service_data,
             touruuid             TYPE sysuuid_x16,
             totalserviceduration TYPE p LENGTH 10 DECIMALS 2,
             totaltimeadjustment  TYPE p LENGTH 10 DECIMALS 2,
           END OF ty_service_data.

    DATA: lt_tour_keys   TYPE STANDARD TABLE OF ty_tour_key,
          ls_tour_key    TYPE ty_tour_key,
          lt_service_agg TYPE STANDARD TABLE OF ty_service_data.

    DATA: lv_tour_uuid    TYPE sysuuid_x16,
          lv_tour_dur_h   TYPE p LENGTH 10 DECIMALS 2,
          lv_driving_h    TYPE p LENGTH 10 DECIMALS 2,

          lv_tour_dur_min TYPE p LENGTH 10 DECIMALS 0,
          lv_driving_min  TYPE p LENGTH 10 DECIMALS 0,

          lv_svc_dur_h    TYPE p LENGTH 10 DECIMALS 2,
          lv_svc_dur_min  TYPE p LENGTH 10 DECIMALS 0,
          lv_svc_adj_min  TYPE p LENGTH 10 DECIMALS 0,

          lv_new_dur_min  TYPE p LENGTH 10 DECIMALS 0,
          lv_rem_min      TYPE p LENGTH 10 DECIMALS 0,
          
          " --- NEW: Variable for Maximum Tour Duration ---
          lv_max_dur      TYPE p LENGTH 10 DECIMALS 3.

    FIELD-SYMBOLS: <ls_orig>             TYPE any,
                   <ls_calc>             TYPE any,
                   <lv_val_uuid>         TYPE any,
                   <lv_val_dur>          TYPE any,
                   <lv_val_drive>        TYPE any,
                   
                   " --- NEW: Field symbols for reading source data ---
                   <lv_val_max_dur>      TYPE any,
                   <lv_val_max_dur_unit> TYPE any,

                   <lv_set_cap>          TYPE any,
                   <lv_set_rem>          TYPE any,
                   <lv_set_tot>          TYPE any,
                   <lv_set_unit>         TYPE any,
                   
                   " --- NEW: Field symbol for setting converted data ---
                   <lv_set_max_conv>     TYPE any.

    " -------------------------------------------------------------------------
    " 2. Collect all Tour UUIDs for our fast DB query
    " -------------------------------------------------------------------------
    LOOP AT it_original_data ASSIGNING <ls_orig>.
      ASSIGN COMPONENT 'TOURUUID' OF STRUCTURE <ls_orig> TO <lv_val_uuid>.
      IF sy-subrc = 0 AND <lv_val_uuid> IS NOT INITIAL.
        ls_tour_key-touruuid = <lv_val_uuid>.
        APPEND ls_tour_key TO lt_tour_keys.
      ENDIF.
    ENDLOOP.

    " Remove duplicates to make the query even faster
    SORT lt_tour_keys BY touruuid.
    DELETE ADJACENT DUPLICATES FROM lt_tour_keys COMPARING touruuid.

    " -------------------------------------------------------------------------
    " 3. Fetch Aggregated Service Data from our new CDS View
    " -------------------------------------------------------------------------
    IF lt_tour_keys IS NOT INITIAL.
      SELECT touruuid,
             totalserviceduration,
             totaltimeadjustment
        FROM zi_tour_svc_sum_wr   " <--- Must match your new CDS view name
        FOR ALL ENTRIES IN @lt_tour_keys
        WHERE touruuid = @lt_tour_keys-touruuid
        INTO TABLE @lt_service_agg.
    ENDIF.

    " -------------------------------------------------------------------------
    " 4. Perform math and assign to virtual fields row-by-row
    " -------------------------------------------------------------------------
    LOOP AT it_original_data ASSIGNING <ls_orig>.
      READ TABLE ct_calculated_data ASSIGNING <ls_calc> INDEX sy-tabix.
      CHECK sy-subrc = 0.

      " --- A. Get Tour Fields ---

      ASSIGN COMPONENT 'DRIVINGTIME'  OF STRUCTURE <ls_orig> TO <lv_val_drive>.
      ASSIGN COMPONENT 'TOURDURATION' OF STRUCTURE <ls_orig> TO <lv_val_dur>.
      ASSIGN COMPONENT 'TOURCAPACITY' OF STRUCTURE <ls_orig> TO <lv_val_dur>. " Note: You might want to check this, it overwrites TOURDURATION assignment
      ASSIGN COMPONENT 'TOURUUID'     OF STRUCTURE <ls_orig> TO <lv_val_uuid>.
      
      " --- NEW: Assign MaximumTourDuration fields ---
      ASSIGN COMPONENT 'MAXIMUMTOURDURATION'     OF STRUCTURE <ls_orig> TO <lv_val_max_dur>.
      ASSIGN COMPONENT 'MAXIMUMTOURDURATIONUNIT' OF STRUCTURE <ls_orig> TO <lv_val_max_dur_unit>.

      IF sy-subrc <> 0. CONTINUE. ENDIF.

      lv_tour_uuid  = <lv_val_uuid>.
      lv_tour_dur_h = COND #( WHEN <lv_val_dur> IS ASSIGNED THEN <lv_val_dur> ELSE 0 ).
      lv_driving_h  = COND #( WHEN <lv_val_drive> IS ASSIGNED THEN <lv_val_drive> ELSE 0 ).

      " --- B. Convert Tour fields to Minutes ---
      lv_tour_dur_min = lv_tour_dur_h * 60.
      lv_driving_min  = lv_driving_h * 60.

      " --- C. Read pre-aggregated Service Data from our DB result ---
      READ TABLE lt_service_agg INTO DATA(ls_agg) WITH KEY touruuid = lv_tour_uuid.
      IF sy-subrc = 0.
        lv_svc_dur_h   = ls_agg-totalserviceduration.
        lv_svc_adj_min = ls_agg-totaltimeadjustment. " Already in minutes
      ELSE.
        CLEAR: lv_svc_dur_h, lv_svc_adj_min.
      ENDIF.

      " Convert Service TotalDuration to Minutes
      lv_svc_dur_min = lv_svc_dur_h * 60.

      " --- D. THE MATH ---
      " 1. NewDuration = TotalDuration(Min) + DrivingTime(Min) + zz_timeadjustment(Min)
      lv_new_dur_min = lv_svc_dur_min + lv_driving_min + lv_svc_adj_min.

      " 2. Remaining = TourDuration(Min) - NewDuration
      lv_rem_min = lv_tour_dur_min - lv_new_dur_min.

      " --- E. Map to UI Virtual Fields ---

      " Map 1: tour_capacity_new = NewDuration
      ASSIGN COMPONENT 'TOUR_CAPACITY_NEW' OF STRUCTURE <ls_calc> TO <lv_set_cap>.
      IF sy-subrc = 0. <lv_set_cap> = lv_new_dur_min. ENDIF.

      " Map 2: tour_duration_min = TourDuration (Target for Progress Bar)
      ASSIGN COMPONENT 'TOUR_DURATION_MIN' OF STRUCTURE <ls_calc> TO <lv_set_tot>.
      IF sy-subrc = 0. <lv_set_tot> = lv_tour_dur_min. ENDIF.

      " Map 3: zz_remaining_duration = Remaining Time
      ASSIGN COMPONENT 'ZZ_REMAINING_DURATION' OF STRUCTURE <ls_calc> TO <lv_set_rem>.
      IF sy-subrc = 0. <lv_set_rem> = lv_rem_min. ENDIF.

      " Map 4: Force Unit to 'MIN' for the UI
      ASSIGN COMPONENT 'ZZ_DURATION_UNIT' OF STRUCTURE <ls_calc> TO <lv_set_unit>.
      IF sy-subrc = 0. <lv_set_unit> = 'MIN'. ENDIF.
      
      " --- NEW: Map 5: Convert and map MaximumTourDuration ---
      ASSIGN COMPONENT 'MAXIMUMTOURDURATION_CONVERTED' OF STRUCTURE <ls_calc> TO <lv_set_max_conv>.
      IF sy-subrc = 0 AND <lv_val_max_dur> IS ASSIGNED AND <lv_val_max_dur_unit> IS ASSIGNED.
        lv_max_dur = <lv_val_max_dur>.
        IF <lv_val_max_dur_unit> = 'H'.
          <lv_set_max_conv> = lv_max_dur * 60.
        ELSE.
          " If it is already in MIN or another unit, map it directly
          <lv_set
