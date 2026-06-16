class /plce/cl_geocode_base definition
  public
  inheriting from /plce/cl_geo_base
  create public .

  public section.
    methods constructor.
    interfaces /plce/if_geocode .
  protected section.
    constants:
      c_appl_log_subobject_name type cl_bali_header_setter=>ty_subobject value '/PLCE/BASE_GEOCODING'.

    methods: geoCodeAddress
      importing par_address type ref to /plce/if_geocode~georequest
                par_result  type ref to /plce/if_geocode~georesult
      raising   /plce/cx_geo.
  private section.
ENDCLASS.



CLASS /PLCE/CL_GEOCODE_BASE IMPLEMENTATION.


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


  method /plce/if_geocode~init.
    init( RFCDestination = RFCDestination
      CommunicationScenarioId = CommunicationScenarioId
      CommunicationServiceId = CommunicationServiceId
      CommunicationSystemId = CommunicationSystemId
      Parameters = Parameters ).
  endmethod.


  method constructor.
    super->constructor( ).
    fappl_log_subobject_name = c_appl_log_subobject_name.
  endmethod.


  method geoCodeAddress.
    " implement in child
  endmethod.
ENDCLASS.
