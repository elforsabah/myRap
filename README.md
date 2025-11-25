CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Tour RESULT result.

    METHODS createtour FOR MODIFY
      IMPORTING keys FOR ACTION Tour~createtour RESULT result.

ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD createtour.

      " call tour->assign services per tour (grouped)
    loop at keys assigning field-symbol(<key>) group by (  template = <key>-%param-tour_template date = <key>-%param-start_date ) assigning field-symbol(<group>).

      "create new tour from template
      modify entities of /PLCE/R_PDTour
        entity Tour
          execute createTourWithTemplate auto fill cid
          with value #( ( %param-without_draft = 'X' %param-tour_template =  <group>-template %param-start_date = <group>-date ) )
            mapped data(mapped_tour)
            failed data(failed_tour)
            reported data(reported_tour).

      if failed_tour is initial.
        read entities of /PLCE/R_PDTour entity Tour
            fields ( TourId ) with corresponding #( mapped_tour-tour )
            result data(tours).
        insert new  /plce/cx_pd_exception( textid = /plce/cx_pd_exception=>tour_confirmed
                                           severity = if_abap_behv_message=>severity-success
                                           tour = tours[ 1 ]-TourId ) into table reported-%other.
      endif.
    endloop.

*    if failed_tour is not initial.
*      failed-service = value #( for k in keys ( %tky = k-%tky ) ).
*      /plce/cl_pd_messagehelper=>collect_reported_tour( exporting messages = reported_tour changing result = reported-%other ).
*    endif.
*
    " results
    read entities of /PLCE/R_PDTour in local mode
      entity Tour
      all fields with corresponding #( keys )
      result data(tour_result).

*    result = value #( for tour  in tour_result ( %tky = tour-%tky %param = service ) ).
    result = value #( for tour  in tour_result ( %tky = tour-%tky %param = service ) ).

  ENDMETHOD.

ENDCLASS.
