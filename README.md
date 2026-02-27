
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

    DATA lv_first_date   TYPE /plce/date.
    DATA lv_last_date    TYPE /plce/date.
    DATA lv_current_date TYPE /plce/date.
    DATA lv_exists       TYPE abap_bool. " Moved outside the loop

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

        " -- FIX: Explicitly clear the variable on EVERY day's iteration
        CLEAR lv_exists.

        " Note: Adjust the fields 'TourTemplate' and 'StartDate' to match your CDS View /PLCE/R_PDTour
        SELECT SINGLE @abap_true
          FROM /plce/r_pdtour
          WHERE TourTemplate = @<group>-template
            AND StartDate    = @lv_current_date
          INTO @lv_exists.

        IF lv_exists = abap_true.
          " -- NEW: Skip creation and log it in the protocol with the date
          APPEND new_message(
                   id       = 'Z_MSG_CL_SERVICE_EXT' " Your message class
                   number   = '008' " Create a MSG: 'Skipped: Tour already exists for &1'
                   severity = if_abap_behv_message=>severity-information " or warning
                   v1       = |{ lv_current_date DATE = USER }|
                   v2       = <group>-template
                 ) TO reported-%other.

        ELSE.
          " -- EXISTING LOGIC: Create the tour because it does not exist yet
          " unique CID for this inner action call
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
              " -- CHANGED: Output success message containing the Date & Tour ID without leading zeros
              APPEND new_message(
                       id       = 'Z_MSG_CL_SERVICE_EXT'
                       number   = '007' " Create a MSG: 'Success: Tour created for &1'
                       severity = if_abap_behv_message=>severity-success
                       v1       = |{ tours[ 1 ]-TourId ALPHA = OUT }|
                       v2       = |{ lv_current_date DATE = USER }|
                     ) TO reported-%other.

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
  ENDMETHOD.


  METHOD precheck_createtour.
    " Today (system date)
    DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

      DATA lv_failed   TYPE abap_bool VALUE abap_false.
      DATA lv_start    TYPE /plce/date.
      DATA lv_end      TYPE /plce/date.
      DATA lv_template TYPE /plce/pdtour_template.

      lv_start    = <key>-%param-start_date.
      lv_end      = <key>-%param-end_date.
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
      IF lv_template IS INITIAL.
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


extension implementation in class zbp_e_bp_r_pdtour unique;

extend behavior for Tour
{

 static action  ( precheck )  createtour parameter ZAE_D_TOURTOURTEMPLATE_AB result [0..*] $self;

 determination assign_earliest_to_latest_date on modify { create; update; }


}

extend behavior for ExtCustom
{

 mapping for /PLCE/TPDTOURCST  {
    zz_remaining_duration = zz_remaining_duration;
 }

field ( readonly : update ) zz_remaining_duration;
}
