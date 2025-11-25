METHOD createtour.
  " call tour->assign services per tour (grouped)
  LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>) GROUP BY ( template = <key>-%param-tour_template date = <key>-%param-start_date ) ASSIGNING FIELD-SYMBOL(<group>).
    "create new tour from template
    MODIFY ENTITIES OF /PLCE/R_PDTour
      ENTITY Tour
      EXECUTE createTourWithTemplate AUTO FILL CID WITH VALUE #( ( %param-without_draft = 'X' %param-tour_template = <group>-template %param-start_date = <group>-date ) )
      MAPPED DATA(mapped_tour)
      FAILED DATA(failed_tour)
      REPORTED DATA(reported_tour).
    IF failed_tour IS INITIAL.
      READ ENTITIES OF /PLCE/R_PDTour
        ENTITY Tour
        FIELDS ( TourId ) WITH CORRESPONDING #( mapped_tour-tour )
        RESULT DATA(tours).
      INSERT NEW /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>tour_confirmed severity = if_abap_behv_message=>severity-success tour = tours[ 1 ]-TourId ) INTO TABLE reported-%other.
    ENDIF.
  ENDLOOP.
* if failed_tour is not initial.
*   failed-service = value #( for k in keys ( %tky = k-%tky ) ).
*   /plce/cl_pd_messagehelper=>collect_reported_tour( exporting messages = reported_tour changing result = reported-%other ).
* endif.
  " results
  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
    ALL FIELDS WITH CORRESPONDING #( mapped_tour-tour )
    RESULT DATA(tour_result).
  result = VALUE #( FOR tour IN tour_result ( %tky = CORRESPONDING #( tour ) %param = tour ) ).
ENDMETHOD.
