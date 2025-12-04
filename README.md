METHOD createtour.

  DATA lv_first_date   TYPE /plce/date.
  DATA lv_last_date    TYPE /plce/date.
  DATA lv_current_date TYPE /plce/date.

  " One call of the static action can contain multiple parameter rows (invocationGrouping = #CHANGE_SET)
  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

    " --- 1) Determine date range from parameter ------------------------
    lv_first_date = <key>-%param-start_date.

    " If no end date was entered, use start date (create just one tour)
    lv_last_date  = COND /plce/date(
                       WHEN <key>-%param-end_date IS INITIAL
                       THEN lv_first_date
                       ELSE <key>-%param-end_date ).

    " Safety: if user swapped dates, flip them
    IF lv_last_date < lv_first_date.
      DATA(lv_tmp) = lv_first_date.
      lv_first_date = lv_last_date.
      lv_last_date  = lv_tmp.
    ENDIF.

    lv_current_date = lv_first_date.

    " --- 2) Create one tour per day in the range ----------------------
    WHILE lv_current_date <= lv_last_date.

      " Inner action: create tour from template
      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          EXECUTE createTourWithTemplate
          FROM VALUE #(
            ( %cid                 = <key>-%cid        " reuse outer CID!
              %param-without_draft = 'X'
              %param-tour_template = <key>-%param-tour_template
              %param-start_date    = lv_current_date ) )
        MAPPED   DATA(mapped_tour)
        FAILED   DATA(failed_tour)
        REPORTED DATA(reported_tour_inner).

      IF failed_tour IS INITIAL.

        " Read created tour(s) to build result & messages
        READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour
            ALL FIELDS
            WITH CORRESPONDING #( mapped_tour-tour )
          RESULT DATA(lt_tours).

        IF lt_tours IS NOT INITIAL.

          " Example success message (keep your existing exception if you like)
          INSERT NEW /plce/cx_pd_exception(
                     textid   = /plce/cx_pd_exception=>tour_confirmed
                     severity = if_abap_behv_message=>severity-success
                     tour     = lt_tours[ 1 ]-TourId )
            INTO TABLE reported-%other.

          " --- 3) Fill static action RESULT --------------------------
          " VERY IMPORTANT for static actions:
          " - %cid MUST be copied from the request (<key>-%cid)
          " - %param contains the actual Tour data returned to the UI
          " - %tky is nice for internal consistency, but not strictly required
          result = VALUE #( BASE result
                            FOR ls_tour IN lt_tours
                            ( %cid   = <key>-%cid
                              %tky   = ls_tour-%tky
                              %param = ls_tour ) ).
        ENDIF.

      ELSE.
        " Optional: bubble up inner reported/failed to outer action tables
        " APPEND LINES OF failed_tour-tour   TO failed-tour.
        " APPEND LINES OF reported_tour_inner-tour TO reported-tour.
      ENDIF.

      " next day
      lv_current_date = lv_current_date + 1.

    ENDWHILE.

  ENDLOOP.

  " *** IMPORTANT ***
  " Remove the old final READ / result assignment:
  "   READ ENTITIES ... WITH CORRESPONDING #( keys ) ...
  "   result = VALUE #( ... )
  "
  " For a static action, 'keys' does NOT contain Tour keys,
  " and that code overwrote the correctly built 'result'.

ENDMETHOD.
