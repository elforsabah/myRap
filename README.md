METHOD createtour.

  DATA lv_cid          TYPE abp_behv_cid.
  DATA lv_first_date   TYPE /plce/date.
  DATA lv_last_date    TYPE /plce/date.
  DATA lv_current_date TYPE /plce/date.

  "Holds all (template,date) combinations that must NOT be created
  TYPES: BEGIN OF ty_exist_key,
           tour_template TYPE /plce/pdtour_template,
           start_date    TYPE /plce/date,
         END OF ty_exist_key.

  DATA lt_exist TYPE HASHED TABLE OF ty_exist_key
                WITH UNIQUE KEY tour_template start_date.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
       GROUP BY ( template   = <key>-%param-tour_template
                  first_date = <key>-%param-start_date
                  last_date  = <key>-%param-end_date )
       ASSIGNING FIELD-SYMBOL(<group>).

    "1) Determine date range for this request
    lv_first_date = <group>-first_date.
    lv_last_date  = COND /plce/date(
                      WHEN <group>-last_date IS INITIAL
                      THEN lv_first_date
                      ELSE <group>-last_date ).

    IF lv_last_date < lv_first_date.
      DATA(lv_tmp) = lv_first_date.
      lv_first_date = lv_last_date.
      lv_last_date  = lv_tmp.
    ENDIF.

    "2) Preload all existing tours for (template, date range) into lt_exist
    DATA lt_db_exist TYPE STANDARD TABLE OF ty_exist_key.

    SELECT tourtemplate AS tour_template
           startdate    AS start_date
      FROM /plce/tpdtour
      WHERE tourtemplate = @<group>-template
        AND startdate    BETWEEN @lv_first_date AND @lv_last_date
      INTO TABLE @lt_db_exist.

    INSERT LINES OF lt_db_exist INTO TABLE lt_exist ACCEPTING DUPLICATE KEYS.

    "3) Create one tour per day only if it does NOT already exist for (template,date)
    lv_current_date = lv_first_date.

    WHILE lv_current_date <= lv_last_date.

      READ TABLE lt_exist WITH TABLE KEY
        tour_template = <group>-template
        start_date    = lv_current_date
        TRANSPORTING NO FIELDS.

      IF sy-subrc = 0.
        lv_current_date = lv_current_date + 1.
        CONTINUE. "already exists -> skip
      ENDIF.

      lv_cid = cl_system_uuid=>create_uuid_x16_static( ).

      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          EXECUTE createTourWithTemplate
          FROM VALUE #(
            ( %cid                 = lv_cid
              %param-without_draft = 'X'
              %param-tour_template = <group>-template
              %param-start_date    = lv_current_date ) )
        MAPPED   DATA(mapped_tour)
        FAILED   DATA(failed_tour)
        REPORTED DATA(reported_tour).

      IF failed_tour IS INITIAL.

        "Important: block the same (template,date) immediately in this LUW
        INSERT VALUE ty_exist_key(
                 tour_template = <group>-template
                 start_date    = lv_current_date )
          INTO TABLE lt_exist ACCEPTING DUPLICATE KEYS.

        READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour
            ALL FIELDS
            WITH CORRESPONDING #( mapped_tour-tour )
          RESULT DATA(tours).

        IF lines( tours ) > 0.

          INSERT NEW /plce/cx_pd_exception(
                   textid   = /plce/cx_pd_exception=>tour_confirmed
                   severity = if_abap_behv_message=>severity-success
                   tour     = tours[ 1 ]-TourId )
            INTO TABLE reported-%other.

          result = VALUE #(
                    BASE result
                    FOR tour IN tours
                    ( %cid   = lv_cid
                      %param = tour ) ).
        ENDIF.

      ENDIF.

      lv_current_date = lv_current_date + 1.

    ENDWHILE.

  ENDLOOP.

ENDMETHOD.
