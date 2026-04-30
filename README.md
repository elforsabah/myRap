METHOD get_bms_bearer_token.

  CLEAR: ev_token, ev_error.

  DATA(lv_auth_json) = |\{ "username": "{ iv_username }", | &&
                        |"password": "{ iv_password }" \}|.

  DATA lo_http TYPE REF TO if_http_client.

  cl_http_client=>create_by_url(
    EXPORTING url    = iv_base_url && '/api/user/authenticate/user'
    IMPORTING client = lo_http
    EXCEPTIONS OTHERS = 4 ).

  IF sy-subrc <> 0.
    ev_error = 'BMS: auth HTTP client creation failed'.
    RETURN.
  ENDIF.

  lo_http->request->set_method( if_http_request=>co_request_method_post ).
  lo_http->request->set_header_field(
    name  = 'Content-Type'
    value = 'application/json' ).
  lo_http->request->set_cdata( lv_auth_json ).

  lo_http->send(    EXCEPTIONS OTHERS = 4 ).
  lo_http->receive( EXCEPTIONS OTHERS = 4 ).

  DATA lv_code TYPE i.
  lo_http->response->get_status( IMPORTING code = lv_code ).
  DATA(lv_body) = lo_http->response->get_cdata( ).
  lo_http->close( ).

  IF lv_code <> 200.
    ev_error = |BMS auth failed — HTTP { lv_code }: { lv_body }|.
    RETURN.
  ENDIF.

  TYPES: BEGIN OF ty_auth,
           token TYPE string,
         END OF ty_auth.
  DATA ls_auth TYPE ty_auth.

  /ui2/cl_json=>deserialize(
    EXPORTING json = lv_body
    CHANGING  data = ls_auth ).

  IF ls_auth-token IS INITIAL.
    ev_error = |BMS auth: no token in response. Body: { lv_body }|.
    RETURN.
  ENDIF.

  " Token ~373 chars; with 'Bearer ' prefix = 380 chars
  ev_token = |Bearer { ls_auth-token }|.

ENDMETHOD.

METHOD post_bms_order.

  CLEAR: ev_http_status, ev_response.

  DATA lo_http TYPE REF TO if_http_client.

  cl_http_client=>create_by_url(
    EXPORTING url    = iv_base_url && '/api/container/create-order-halle'
    IMPORTING client = lo_http
    EXCEPTIONS OTHERS = 4 ).

  IF sy-subrc <> 0.
    ev_http_status = 0.
    ev_response    = 'BMS: order HTTP client creation failed'.
    RETURN.
  ENDIF.

  lo_http->request->set_method( if_http_request=>co_request_method_post ).
  lo_http->request->set_header_field(
    name  = 'Content-Type'
    value = 'application/json-patch+json' ).
  lo_http->request->set_header_field(
    name  = 'Authorization'
    value = iv_bearer_token ).
  lo_http->request->set_cdata( iv_json ).

  lo_http->send(    EXCEPTIONS OTHERS = 4 ).
  lo_http->receive( EXCEPTIONS OTHERS = 4 ).

  DATA lv_code TYPE i.
  lo_http->response->get_status( IMPORTING code = lv_code ).
  ev_http_status = lv_code.
  ev_response    = lo_http->response->get_cdata( ).

  lo_http->close( ).

ENDMETHOD.
