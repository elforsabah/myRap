METHOD createtour.
  DATA lv_cid TYPE abp_behv_cid.
  DATA lv_first_date TYPE /plce/date.
  DATA lv_last_date TYPE /plce/date.
  DATA lv_current_date TYPE /plce/date.
  DATA tours TYPE STANDARD TABLE OF /PLCE/R_PDTour. " Adjust to exact result type if needed; collect all created tours per group

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>) 
    GROUP BY ( template = <key>-%param-tour_template
               first_date = <key>-%param-start_date
               last_date = <key>-%param-end_date ) 
    ASSIGNING FIELD-SYMBOL(<group>).

    " 1) Determine date range for this request
    lv_first_date = <group>-first_date.
    " If no last_date was entered, use first_date (create just one tour)
    lv_last_date = COND /plce/date( WHEN <group>-last_date IS INITIAL THEN lv_first_date ELSE <group>-last_date ).
    " Simple safety: if user swapped dates, flip them
    IF lv_last_date < lv_first_date.
      DATA(lv_tmp) = lv_first_date.
      lv_first_date = lv_last_date.
      lv_last_date = lv_tmp.
    ENDIF.
    lv_current_date = lv_first_date.

    CLEAR: tours.

    DATA has_failure TYPE abap_bool VALUE abap_false.

    " 2) Create one tour per day in the range
    WHILE lv_current_date <= lv_last_date.
      " unique CID for this inner action call
      lv_cid = cl_system_uuid=>create_uuid_x16_static( ).

      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour 
        EXECUTE createTourWithTemplate 
        FROM VALUE #( ( %cid = lv_cid
                        %param-without_draft = 'X'
                        %param-tour_template = <group>-template
                        %param-start_date = lv_current_date ) )
        MAPPED DATA(mapped_tour)
        FAILED DATA(failed_tour)
        REPORTED DATA(reported_tour).

      IF failed_tour IS NOT INITIAL.
        has_failure = abap_true.
        " Optionally collect and forward reported_tour messages to outer reported-tour
        reported = CORRESPONDING #( DEEP reported_tour ).
        " Continue to try other days or break if all-or-nothing
      ELSE.
        " Read created tour(s) using mapped %tky for precise key matching
        READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour 
          ALL FIELDS 
          WITH VALUE #( FOR m IN mapped_tour-tour ( m-%tky ) )
          RESULT DATA(created_tours).
        IF lines( created_tours ) > 0.
          APPEND LINES OF created_tours TO tours.
        ENDIF.
      ENDIF.

      " next day
      lv_current_date = lv_current_date + 1.
    ENDWHILE.

    " 3) After all creations for the group, handle results and messages
    IF has_failure = abap_true.
      " Set failed for all members in group if any day failed (adjust logic as needed)
      LOOP AT GROUP <group> ASSIGNING FIELD-SYMBOL(<member>).
        APPEND VALUE #( %cid = <member>-%cid ) TO failed-tour.
      ENDLOOP.
    ENDIF.

    IF tours IS NOT INITIAL.
      " Success messages tied to created entities (%tky explicit to avoid component error)
      LOOP AT tours ASSIGNING FIELD-SYMBOL(<tour>).
        APPEND VALUE #( %tky = VALUE #( TourId = <tour>-TourId )  " Explicit %tky construction (add other keys if composite)
                        %msg = NEW /plce/cx_pd_exception( textid   = /plce/cx_pd_exception=>tour_confirmed
                                                           severity = if_abap_behv_message=>severity-success
                                                           tour     = <tour>-TourId ) )
          TO reported-tour.
      ENDLOOP.

      " Assign created tours to each input %cid in the group
      LOOP AT GROUP <group> ASSIGNING FIELD-SYMBOL(<member>).
        LOOP AT tours INTO DATA(ls_tour).  " One entry per successful tour
          result = VALUE #( BASE result 
                            ( %cid_ref = <member>-%cid
                              %tky     = VALUE #( TourId = ls_tour-TourId )  " Explicit VALUE fixes "%tky" component error
                              %param   = ls_tour ) ).
        ENDLOOP.
      ENDLOOP.
    ENDIF.
  ENDLOOP.
ENDMETHOD.
