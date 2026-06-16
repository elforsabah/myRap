  method /plce/if_geocode~geocode.
    data:
      lresult   type ref to /plce/if_geocode~georesult,
      lt_result type /plce/if_geocode~georesults.
    try.
        fclient = get_http_client( par_rfc_destination = frfc_destination par_comm_system_id = fcomm_system_id par_scenario_id = fscenario_id par_service_id = fservice_id ).
      catch /plce/cx_geo into data(lex).
        loop at par_addresses reference into data(laddress).
          append initial line to par_results reference into lresult.
          lresult->error = lex->get_text( ).
          lresult->success = abap_false.
        endloop.
    endtry.
    if fclient is initial.
      return.
    endif.
    loop at par_addresses reference into laddress.
      append initial line to par_results reference into lresult.
      lresult->key = laddress->key.
      try.
          geoCodeAddress( par_address = laddress par_result = lresult ).
        catch /plce/cx_geo into lex.
          lresult->error = lex->get_text( ).
          lresult->success = abap_false.
      endtry.
    endloop.
  endmethod.
