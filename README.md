METHOD createtour.

    DATA lv_first_date   TYPE /plce/date.
    DATA lv_last_date    TYPE /plce/date.
    DATA lv_current_date TYPE /plce/date.
    DATA lv_exists       TYPE abap_bool. " Moved outside the loop

    " 1) Define a helper table to store the date alongside the message
    TYPES: BEGIN OF ty_msg_sort,
             date TYPE /plce/date,
             msg  TYPE REF TO if_abap_behv_message,
           END OF ty_msg_sort.
    DATA lt_msg_sort TYPE STANDARD TABLE OF ty_msg_sort.

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
        DATA(lv_tmp)  = lv_first_date.
        lv_first_date = lv_last_date.
        lv_last_date  = lv_tmp.
      ENDIF.

      lv_current_date = lv_first_date.

      " 2) Create one tour per day in the range
      WHILE lv_current_date <= lv_last_date.

        " Explicitly clear the variable on EVERY day's iteration
        CLEAR lv_exists.


        SELECT SINGLE @abap_true
          FROM /plce/r_pdtour
          WHERE TourTemplate = @<group>-template
            AND StartDate    = @lv_current_date
          INTO @lv_exists.

        IF lv_exists = abap_true.

          " 2) Append to the helper table instead of reported-%other directly
          APPEND VALUE #( date = lv_current_date
                          msg  = new_message(
                                   id       = 'Z_MSG_CL_SERVICE_EXT' " Your message class
                                   number   = '008' "
                                   severity = if_abap_behv_message=>severity-information " or warning
                                   v1       = |{ lv_current_date DATE = USER }|
                                   v2       = <group>-template
                                 ) 
                        ) TO lt_msg_sort.

        ELSE.

          DATA(lv_internal_cid) = cl_system_uuid=>create_uuid_x16_static( ).

          MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              EXECUTE createTourWithTemplate
              FROM VALUE #(
                ( %cid                 = lv_internal_cid
                  %param-without_draft = 'X'
                  %param-tour_template = <group>-template
                  %param-start_date    = lv_current_date ) )
            MAPPED   DATA(mapped_tour)
            FAILED   DATA(failed_tour)
            REPORTED DATA(reported_tour).

            " Read created tour(s) to build result & message
            READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
              ENTITY Tour
                ALL FIELDS
                WITH CORRESPONDING #( mapped_tour-tour )
              RESULT DATA(tours).

            IF lines( tours ) > 0.
              " 3) Append to the helper table instead of reported-%other directly
              APPEND VALUE #( date = lv_current_date
                              msg  = new_message(
                                       id       = 'Z_MSG_CL_SERVICE_EXT'
                                       number   = '007' " Create a MSG: 'Success: Tour created for &1'
                                       severity = if_abap_behv_message=>severity-success
                                       v1       = |{ lv_current_date DATE = USER }|
                                       v2       = |{ tours[ 1 ]-TourId ALPHA = OUT }|
                                     ) 
                            ) TO lt_msg_sort.

              " Static action result: append all created tours using the ORIGINAL UI CID
              result = VALUE #( BASE result
                                FOR tour IN tours
                                (
                                 %cid   = lv_internal_cid
                                 %param = tour ) ).
            ENDIF.
        ENDIF.

        " next day
        lv_current_date = lv_current_date + 1.

      ENDWHILE.
    ENDLOOP.

    " 4) Sort the helper table by the date we saved
    SORT lt_msg_sort BY date ASCENDING.

    " 5) Move the sorted messages into reported-%other
    LOOP AT lt_msg_sort INTO DATA(ls_msg_sort).
      APPEND ls_msg_sort-msg TO reported-%other.
    ENDLOOP.

  ENDMETHOD.
