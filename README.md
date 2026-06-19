CLASS zcl_wr_pd_tour_helper DEFINITION
  PUBLIC
  CREATE PUBLIC.

  PUBLIC SECTION.

    CLASS-METHODS get_bms_bearer_token
      IMPORTING
        iv_base_url  TYPE string
        iv_username  TYPE string
        iv_password  TYPE string
      EXPORTING
        ev_token     TYPE string
        ev_error     TYPE string.

    CLASS-METHODS post_bms_order
      IMPORTING
        iv_base_url     TYPE string
        iv_bearer_token TYPE string
        iv_json         TYPE string
      EXPORTING
        ev_http_status  TYPE i
        ev_response     TYPE string.

    CLASS-METHODS storno_bms_order
      IMPORTING
        iv_base_url     TYPE string
        iv_bearer_token TYPE string
        iv_order_number TYPE string
        iv_full_json    TYPE string
      EXPORTING
        ev_http_status  TYPE i
        ev_response     TYPE string.

    CLASS-METHODS log_bms_call
      IMPORTING
        iv_tour_uuid    TYPE /plce/pdtour_uuid
        iv_service_uuid TYPE /plce/pdservice_uuid
        iv_order_number TYPE aufnr
        iv_endpoint     TYPE string
        iv_http_status  TYPE i
        iv_request      TYPE string
        iv_response     TYPE string.

ENDCLASS.

CLASS zcl_wr_pd_tour_helper IMPLEMENTATION.

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

    TYPES: BEGIN OF ty_auth_data,
             access_token TYPE string,
           END OF ty_auth_data,
           BEGIN OF ty_auth_response,
             data TYPE ty_auth_data,
           END OF ty_auth_response.
    DATA ls_auth TYPE ty_auth_response.

    /ui2/cl_json=>deserialize(
      EXPORTING
        json        = lv_body
        pretty_name = /ui2/cl_json=>pretty_mode-camel_case
      CHANGING
        data        = ls_auth ).

    IF ls_auth-data-access_token IS INITIAL.
      ev_error = |BMS auth: no accessToken in response. Body: { lv_body }|.
      RETURN.
    ENDIF.

    ev_token = |Bearer { ls_auth-data-access_token }|.

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

  METHOD storno_bms_order.

    CLEAR: ev_http_status, ev_response.

    " Build JSON — minimal storno if no full payload provided,
    " otherwise replace status field in the existing JSON
    DATA(lv_json) = COND string(
      WHEN iv_full_json IS INITIAL
      THEN |\{ "status": "STORNIERT", "orderNumber": "{ iv_order_number }" \}|
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

  METHOD log_bms_call.

    DATA ls_log TYPE zbms_api_log.
    ls_log-log_uuid        = cl_system_uuid=>create_uuid_x16_static( ).
    ls_log-created_at      = cl_abap_context_info=>get_system_time_stamp( ).
    ls_log-created_by      = sy-uname.
    ls_log-direction       = 'OUTBOUND'.
    ls_log-tour_uuid       = iv_tour_uuid.
    ls_log-service_uuid    = iv_service_uuid.
    ls_log-order_number    = iv_order_number.
    ls_log-endpoint        = iv_endpoint.
    ls_log-http_status     = iv_http_status.
    ls_log-request_payload = iv_request.
    ls_log-response_body   = iv_response.
    INSERT zbms_api_log FROM ls_log.

  ENDMETHOD.

ENDCLASS.
