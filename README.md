METHOD storno_bms_order.

  CLEAR: ev_http_status, ev_response.

  " If BMS confirms minimal storno (orderNumber + status only):
  DATA(lv_json) = COND string(
    WHEN iv_full_json IS INITIAL
    " Minimal storno payload — pending BMS confirmation
    THEN |\{ "status": "STORNIERT", "orderNumber": "{ iv_order_number }" \}|
    " Full payload storno — replace status field in existing JSON
    ELSE iv_full_json ).

  IF iv_full_json IS NOT INITIAL.
    REPLACE FIRST OCCURRENCE OF '"status":"OK"'
      IN lv_json WITH '"status":"STORNIERT"'.
  ENDIF.

  DATA lo_http TYPE REF TO if_http_client.

  cl_http_client=>create_by_url(
    EXPORTING url    = iv_base_url && '/api/container/create-order-halle'
    IMPORTING client = lo_http
    EXCEPTIONS OTHERS = 4 ).

  IF sy-subrc <> 0.
    ev_http_status = 0.
    ev_response    = 'BMS Storno: HTTP client creation failed'.
    RETURN.
  ENDIF.

  lo_http->request->set_method( if_http_request=>co_request_method_post ).
  lo_http->request->set_header_field(
    name  = 'Content-Type'
    value = 'application/json-patch+json' ).
  lo_http->request->set_header_field(
    name  = 'Authorization'
    value = iv_bearer_token ).
  lo_http->request->set_cdata( lv_json ).

  lo_http->send(    EXCEPTIONS OTHERS = 4 ).
  lo_http->receive( EXCEPTIONS OTHERS = 4 ).

  DATA lv_code TYPE i.
  lo_http->response->get_status( IMPORTING code = lv_code ).
  ev_http_status = lv_code.
  ev_response    = lo_http->response->get_cdata( ).

  lo_http->close( ).

ENDMETHOD.
