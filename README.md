Es wurde gewünscht, dass in der neuen Funktion "Tour anlegen" geprüft wird, ob zu einem Tag innerhalb des gewählten Zeitraums schon eine Tagestour zur Tourvorlage existiert. In dem Fall das Anlegen für das Datum bitte überspringen und das im angezeigten Protokoll vermerken.
Im Protokoll bitte für alle Einträge soll jeweils das Datum je angelegter (oder übersprungener) Tour mit angezeigt werden.
Nach dem Anlegen der Touren (ggf. nach Bestätigen des Protokolls) soll die Ansicht möglichst gleich automatisch aktualisiert werden, sodass die angelegten Touren in der Liste zu sehen sind.


CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Tour RESULT result.

    METHODS createtour FOR MODIFY
      IMPORTING keys FOR ACTION Tour~createtour RESULT result.

    METHODS precheck_createtour FOR PRECHECK
      IMPORTING keys FOR ACTION Tour~createtour .


    METHODS assign_earliest_to_latest_date FOR DETERMINE ON MODIFY
    IMPORTING keys FOR Tour~assign_earliest_to_latest_date.
ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.


METHOD assign_earliest_to_latest_date.

  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      FIELDS ( StartDate EndDate )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tour).

  MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      UPDATE FIELDS ( EndDate )
      WITH VALUE #(
        FOR ls IN lt_tour
        WHERE ( StartDate IS NOT INITIAL AND EndDate IS INITIAL )
        ( %tky    = ls-%tky
          EndDate = ls-StartDate )
      )
    FAILED   DATA(lt_fai)
    REPORTED DATA(lt_rep).

ENDMETHOD.

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
                              (
                               %cid = lv_cid
                               %param = tour ) ).
          ENDIF.

        " next day
        lv_current_date = lv_current_date + 1.

      ENDWHILE.
    ENDLOOP.
  ENDMETHOD.

  METHOD precheck_createtour.

    " Today (system date)
    DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

      DATA lv_failed TYPE abap_bool VALUE abap_false.
      DATA lv_start  TYPE /plce/date.
      DATA lv_end    TYPE /plce/date.
      DATA lv_template TYPE /plce/pdtour_template.

      lv_start = <key>-%param-start_date.
      lv_end   = <key>-%param-end_date.
      lv_template = <key>-%param-tour_template.

      " unique CID for this inner action call
      DATA(lv_cid) = cl_system_uuid=>create_uuid_x16_static( ).

      " 1) Start date must not be empty
      IF lv_start IS INITIAL.
        APPEND VALUE #(
            %cid = <key>-%cid
            %msg = new_message(
                     id       = 'Z_MSG_CL_SERVICE_EXT'
                     number   = '003'
                     severity = if_abap_behv_message=>severity-error
                     v1       = |{ lv_start DATE = USER }|
                   )
          )
     TO reported-tour.
        lv_failed = abap_true.
      ENDIF.

      " 2) Start date must be <= end date (if end date is given)
      IF lv_end IS NOT INITIAL AND lv_end < lv_start.


        APPEND VALUE #(
      %cid = <key>-%cid
      %msg = new_message(
               id       = 'Z_MSG_CL_SERVICE_EXT'
               number   = '002'
               severity = if_abap_behv_message=>severity-error
               v1       = |{ lv_start DATE = USER }|
               v2       = |{ lv_end DATE = USER }|
             )
    )
TO reported-tour.

        lv_failed = abap_true.
      ENDIF.

      " 3) Tour template must not be empty

      IF lv_template IS INITIAL..

        APPEND VALUE #(
             %cid = <key>-%cid
             %msg = new_message(
                      id       = 'Z_MSG_CL_SERVICE_EXT'
                      number   = '001'
                      severity = if_abap_behv_message=>severity-error
                    )
            )
            TO reported-tour.
                    lv_failed = abap_true.
                  ENDIF.

      " If any check failed, block this action call
      IF lv_failed = abap_true.
        APPEND VALUE #( %cid = <key>-%cid ) TO failed-tour.
      ENDIF.

    ENDLOOP.

  ENDMETHOD.
ENDCLASS.
