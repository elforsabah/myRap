CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Tour RESULT result.

    METHODS createtour FOR MODIFY
      IMPORTING keys FOR ACTION Tour~createtour RESULT result.

    METHODS precheck_createtour FOR PRECHECK
      IMPORTING keys FOR ACTION Tour~createtour .

ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD createtour.

    DATA lv_cid          TYPE abp_behv_cid.
    DATA lv_first_date   TYPE /plce/date.
    DATA lv_last_date    TYPE /plce/date.
    DATA lv_current_date TYPE /plce/date.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
         GROUP BY ( template   = <key>-%param-tour_template
                    first_date = <key>-%param-start_date
                    last_date  = <key>-%param-end_date )
         ASSIGNING FIELD-SYMBOL(<group>).

      " 1) Determine date range for this request
      lv_first_date = <group>-first_date.
      " If no last_date was entered, use first_date (create just one tour)
      lv_last_date  = COND /plce/date(
                         WHEN <group>-last_date IS INITIAL
                         THEN lv_first_date
                         ELSE <group>-last_date ).

      " Simple safety: if user swapped dates, flip them
      IF lv_last_date < lv_first_date.
        DATA(lv_tmp) = lv_first_date.
        lv_first_date = lv_last_date.
        lv_last_date  = lv_tmp.
      ENDIF.

      lv_current_date = lv_first_date.

      " 2) Create one tour per day in the range
      WHILE lv_current_date <= lv_last_date.

        " unique CID for this inner action call
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

        " If inner action failed for this day, you could:
        " - collect messages
        " - optionally set failed-createtour for the outer action
        IF failed_tour IS INITIAL.

          " Read created tour(s) to build result & message
          READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              ALL FIELDS
              WITH CORRESPONDING #( mapped_tour-tour )
            RESULT DATA(tours).

          IF lines( tours ) > 0.
            " Success message per created tour (optional)
            INSERT NEW /plce/cx_pd_exception(
                     textid   = /plce/cx_pd_exception=>tour_confirmed
                     severity = if_abap_behv_message=>severity-success
                     tour     = tours[ 1 ]-TourId )
              INTO TABLE reported-%other.

            " Static action result: append all created tours
            result = VALUE #( BASE result
                              FOR tour IN tours
                              ( %param = tour ) ).
          ENDIF.

        ENDIF.

        " next day
        lv_current_date = lv_current_date + 1.

      ENDWHILE.
    ENDLOOP.

     " results
    read entities of /PLCE/R_PDTour in local mode
      entity Tour
      all fields with corresponding #( keys )
      result data(tour_result).

    result = value #( for tour  in tour_result ( %cid = lv_cid %param = tour ) ).


  ENDMETHOD.
