METHOD createtour.
  DATA lv_cid TYPE abp_behv_cid.
  DATA lv_first_date TYPE /plce/date.
  DATA lv_last_date TYPE /plce/date.
  DATA lv_current_date TYPE /plce/date.
  DATA tours TYPE TABLE FOR READ RESULT /PLCE/R_PDTour\\Tour. " Collect all created tours per group

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>) GROUP BY ( template = <key>-%param-tour_template
                                                        first_date = <key>-%param-start_date
                                                        last_date = <key>-%param-end_date ) ASSIGNING FIELD-SYMBOL(<group>).
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

    CLEAR tours. " Reset for each group

    " 2) Create one tour per day in the range
    WHILE lv_current_date <= lv_last_date.
      " unique CID for this inner action call
      lv_cid = cl_system_uuid=>create_uuid_x16_static( ).

      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour EXECUTE createTourWithTemplate FROM VALUE #( ( %cid = lv_cid
                                                                   %param-without_draft = 'X'
                                                                   %param-tour_template = <group>-template
                                                                   %param-start_date = lv_current_date ) )
        MAPPED DATA(mapped_tour)
        FAILED DATA(failed_tour)
        REPORTED DATA(reported_tour).

      " If inner action failed for this day, you could:
      " - collect messages
      " - optionally set failed-createtour for the outer action
      IF failed_tour IS NOT INITIAL.
        LOOP AT GROUP <group> ASSIGNING FIELD-SYMBOL(<member>).
          APPEND VALUE #( %cid = <member>-%cid ) TO failed-tour.
          " Optionally add error messages to reported-tour with %cid_ref = <member>-%cid
          " For example, transfer reported_tour messages here if needed
        ENDLOOP.
        " You might want to continue or break depending on requirements
      ELSE.
        " Read created tour(s) to build result & message
        READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour ALL FIELDS WITH CORRESPONDING #( mapped_tour-tour )
          RESULT DATA(created_tours).
        IF lines( created_tours ) > 0.
          APPEND LINES OF created_tours TO tours.
        ENDIF.
      ENDIF.

      " next day
      lv_current_date = lv_current_date + 1.
    ENDWHILE.

    " After all creations for the group, assign results and messages to each member
    IF tours IS NOT INITIAL.
      " Success messages tied to reported-tour
      LOOP AT tours ASSIGNING FIELD-SYMBOL(<tour>).
        APPEND VALUE #( %tky = CORRESPONDING #( <tour> )
                        %msg = NEW /plce/cx_pd_exception( textid   = /plce/cx_pd_exception=>tour_confirmed
                                                           severity = if_abap_behv_message=>severity-success
                                                           tour     = <tour>-TourId ) )
          TO reported-tour.
      ENDLOOP.

      " Assign created tours to each input %cid in the group
      LOOP AT GROUP <group> ASSIGNING <member>.
        result = VALUE #( BASE result
                          FOR tour IN tours ( %cid_ref = <member>-%cid
                                              %tky     = CORRESPONDING #( tour )
                                              %param   = tour ) ).
      ENDLOOP.
    ELSEIF failed_tour IS NOT INITIAL. " If any failures, ensure failed is set (already handled per date, but consolidate if needed)
      " Additional handling if all dates failed
    ENDIF.
  ENDLOOP.
ENDMETHOD.
