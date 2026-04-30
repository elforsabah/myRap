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

  "-----------------------------------------------------------------------
  " FIX: BMS response structure is:
  "   { "data": { "accessToken": "eyJ..." } }
  " NOT { "token": "eyJ..." }
  " Two nested types required to deserialize correctly
  "-----------------------------------------------------------------------
  TYPES: BEGIN OF ty_auth_data,
           access_token TYPE string,   " maps to "accessToken"
         END OF ty_auth_data.

  TYPES: BEGIN OF ty_auth_response,
           data TYPE ty_auth_data,
         END OF ty_auth_response.

  DATA ls_auth TYPE ty_auth_response.

  /ui2/cl_json=>deserialize(
    EXPORTING
      json             = lv_body
      pretty_name      = /ui2/cl_json=>pretty_mode-camel_case
    CHANGING
      data             = ls_auth ).

  IF ls_auth-data-access_token IS INITIAL.
    ev_error = |BMS auth: no accessToken in response. Body: { lv_body }|.
    RETURN.
  ENDIF.

  " Token ~373 chars; with 'Bearer ' prefix = 380 chars
  ev_token = |Bearer { ls_auth-data-access_token }|.

ENDMETHOD.
