CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations
      FOR GLOBAL AUTHORIZATION
      IMPORTING
        REQUEST requested_authorizations FOR Tour
      RESULT
        result.

    METHODS createtour
      FOR MODIFY
      IMPORTING
        keys FOR ACTION Tour~createtour
      RESULT
        result.

ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.

  METHOD get_global_authorizations.
    " nothing special yet
  ENDMETHOD.

  METHOD createtour.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
         GROUP BY ( template = <key>-%param-tour_template
                    date     = <key>-%param-start_date )
         ASSIGNING FIELD-SYMBOL(<group>).

      " Create new tour from template (child action)
      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          EXECUTE createTourWithTemplate
          FROM VALUE #(
            ( %param-without_draft = 'X'
              %param-tour_template = <group>-template
              %param-start_date    = <group>-date ) )
        MAPPED   DATA(mapped_tour)
        FAILED   DATA(failed_tour)
        REPORTED DATA(reported_tour).

      " Optionally collect messages coming from the inner action
      " /plce/cl_pd_messagehelper=>collect_reported_tour(
      "   EXPORTING messages = reported_tour
      "   CHANGING  result   = reported-%other ).

      IF failed_tour IS INITIAL.

        " Read created tour(s) to:
        "  - get the TourId for the message
        "  - fill our static action result
        READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour
            ALL FIELDS
            WITH CORRESPONDING #( mapped_tour-tour )
          RESULT DATA(tours).

        IF lines( tours ) > 0.
          " Success message for the first created tour
          INSERT NEW /plce/cx_pd_exception(
                   textid   = /plce/cx_pd_exception=>tour_confirmed
                   severity = if_abap_behv_message=>severity-success
                   tour     = tours[ 1 ]-TourId )
            INTO TABLE reported-%other.
        ENDIF.

        " Build static action result:
        "   -> only %param (no %tky for static actions!)
        result = VALUE #( BASE result
                          FOR tour IN tours
                          ( %param = tour ) ).

      ELSE.
        " Optional: mark the whole action as failed for the caller
        " failed-createtour = VALUE #(
        "   FOR k IN keys ( %cid = k-%cid ) ).
      ENDIF.

    ENDLOOP.

  ENDMETHOD.

ENDCLASS.
