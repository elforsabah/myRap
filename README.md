CLASS zcl_bms_inbound_handler DEFINITION
  PUBLIC FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    INTERFACES if_http_extension.

ENDCLASS.

CLASS zcl_bms_inbound_handler IMPLEMENTATION.

  METHOD if_http_extension~handle_request.

    "-----------------------------------------------------------------------
    " Inbound REST endpoint for BMS → SAP status callbacks.
    " BMS POSTs JSON: { "orderNumber": "...", "status": "BEGONNEN" }
    " SAP looks up the service UUID and updates ZZ_BMS_STATUS.
    " Registered in SICF: /sap/bc/zbms/status_update
    "-----------------------------------------------------------------------

    " Only accept POST
    IF server->request->get_method( ) <> 'POST'.
      server->response->set_status( code = 405 reason = 'Method Not Allowed' ).
      server->response->set_cdata( '{"error":"POST required"}' ).
      RETURN.
    ENDIF.

    " Read and validate body
    DATA(lv_body) = server->request->get_cdata( ).
    IF lv_body IS INITIAL.
      server->response->set_status( code = 400 reason = 'Bad Request' ).
      server->response->set_cdata( '{"error":"Empty body"}' ).
      RETURN.
    ENDIF.

    " Deserialize JSON callback
    " Full BEENDET payload may also carry actualDuration, wasteWeight etc.
    TYPES: BEGIN OF ty_callback,
             order_number       TYPE string,
             status             TYPE string,
             actual_duration    TYPE i,
             waste_weight       TYPE decfloat16,
             weight_unit        TYPE string,
             execution_time     TYPE string,
             signature_obtained TYPE abap_bool,
           END OF ty_callback.
    DATA ls_cb TYPE ty_callback.

    /ui2/cl_json=>deserialize(
      EXPORTING
        json        = lv_body
        pretty_name = /ui2/cl_json=>pretty_mode-camel_case
      CHANGING
        data        = ls_cb ).

    " Validate orderNumber
    IF ls_cb-order_number IS INITIAL.
      server->response->set_status( code = 400 reason = 'Bad Request' ).
      server->response->set_cdata( '{"error":"orderNumber is required"}' ).
      RETURN.
    ENDIF.

    " Validate status value
    IF ls_cb-status <> 'BEGONNEN'
    AND ls_cb-status <> 'BEENDET'
    AND ls_cb-status <> 'STORNIERT'.
      server->response->set_status( code = 400 reason = 'Bad Request' ).
      server->response->set_cdata(
        |\{ "error": "Invalid status '{ ls_cb-status }'. | &&
        |Expected BEGONNEN, BEENDET or STORNIERT" \}| ).
      RETURN.
    ENDIF.

    " Convert orderNumber to internal format (ALPHA=IN)
    DATA lv_smaufnr TYPE aufnr.
    lv_smaufnr = |{ ls_cb-order_number ALPHA = IN }|.

    " Look up POBJNR via EWA_ORDER_OBJECT
    SELECT SINGLE pobjnr
      FROM ewa_order_object
      WHERE smaufnr = @lv_smaufnr
      INTO @DATA(lv_pobjnr).

    IF sy-subrc <> 0.
      server->response->set_status( code = 404 reason = 'Not Found' ).
      server->response->set_cdata(
        |\{ "error": "Order '{ ls_cb-order_number }' not found in SAP" \}| ).
      RETURN.
    ENDIF.

    " Find service UUID via /plce/tpdsrv
    SELECT SINGLE service_uuid
      FROM /plce/tpdsrv
      WHERE reference_int_id = @lv_pobjnr
      INTO @DATA(lv_service_uuid).

    IF sy-subrc <> 0.
      server->response->set_status( code = 404 reason = 'Not Found' ).
      server->response->set_cdata(
        |\{ "error": "Service for order '{ ls_cb-order_number }' not found" \}| ).
      RETURN.
    ENDIF.

    " Update ZZ_BMS_STATUS on /plce/tpdsrvcst
    UPDATE /plce/tpdsrvcst
      SET zz_bms_status = @ls_cb-status
      WHERE service_uuid = @lv_service_uuid.

    IF sy-subrc <> 0.
      " Row does not exist yet — insert it
      DATA ls_srvcst TYPE /plce/tpdsrvcst.
      CLEAR ls_srvcst.
      ls_srvcst-mandt         = sy-mandt.
      ls_srvcst-service_uuid  = lv_service_uuid.
      ls_srvcst-zz_bms_status = ls_cb-status.
      INSERT /plce/tpdsrvcst FROM ls_srvcst.
    ENDIF.

    " Log the inbound call
    zcl_wr_pd_tour_helper=>log_bms_call(
      iv_tour_uuid    = ''
      iv_service_uuid = lv_service_uuid
      iv_order_number = lv_smaufnr
      iv_endpoint     = '/sap/bc/zbms/status_update'
      iv_http_status  = 200
      iv_request      = lv_body
      iv_response     = 'OK' ).

    " Respond with success
    server->response->set_status( code = 200 reason = 'OK' ).
    server->response->set_header_field(
      name  = 'Content-Type'
      value = 'application/json' ).
    server->response->set_cdata(
      |\{ "result": "OK", | &&
      |"orderNumber": "{ ls_cb-order_number }", | &&
      |"status": "{ ls_cb-status }" \}| ).

  ENDMETHOD.

ENDCLASS.
