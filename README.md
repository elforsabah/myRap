  METHOD post_bms_order.
*   IMPORTING iv_destination  TYPE rfcdest
*             iv_bearer_token TYPE string
*             iv_json         TYPE string
*   EXPORTING ev_http_status  TYPE i
*             ev_response     TYPE string

    CLEAR: ev_http_status, ev_response.

    DATA lo_http TYPE REF TO if_http_client.

    cl_http_client=>create_by_destination(
      EXPORTING
        destination              = iv_destination
      IMPORTING
        client                   = lo_http
      EXCEPTIONS
        argument_not_found       = 1
        destination_not_found    = 2
        destination_no_authority = 3
        plugin_not_active        = 4
        internal_error           = 5
        OTHERS                   = 6 ).

    IF sy-subrc <> 0.
      ev_response = |Destination { iv_destination } nicht nutzbar (subrc { sy-subrc })|.
      RETURN.
    ENDIF.

    cl_http_utility=>set_request_uri(
      request = lo_http->request
      path    = '/BmsApiSapTest/api/container/create-order-halle' ).

    lo_http->request->set_method( if_http_request=>co_request_method_post ).
    lo_http->request->set_content_type( 'application/json' ).
    lo_http->request->set_header_field( name  = 'Authorization'
                                        value = iv_bearer_token ).
    lo_http->request->set_cdata( iv_json ).

    lo_http->send( EXCEPTIONS OTHERS = 4 ).
    IF sy-subrc <> 0.
      lo_http->get_last_error( IMPORTING message = ev_response ).
      lo_http->close( ).
      RETURN.
    ENDIF.

    lo_http->receive( EXCEPTIONS OTHERS = 4 ).
    IF sy-subrc <> 0.
      lo_http->get_last_error( IMPORTING message = ev_response ).
      lo_http->close( ).
      RETURN.
    ENDIF.

    lo_http->response->get_status( IMPORTING code = ev_http_status ).
    ev_response = lo_http->response->get_cdata( ).
    lo_http->close( ).

  ENDMETHOD.
