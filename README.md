METHOD get_bms_bearer_token.

    CLEAR: ev_token, ev_error.

    DATA(lv_auth_json) = |\{ "username": "{ iv_username }", | &&
                          |"password": "{ iv_password }" \}|.

    DATA lo_client   TYPE REF TO if_web_http_client.
    DATA lo_response TYPE REF TO if_web_http_response.

    TRY.
        " CREATE_BY_URL needs no destination provider —
        " available from S/4HANA 2021 onwards
        lo_client = cl_web_http_client_manager=>create_by_url(
                      i_url = iv_base_url && '/api/user/authenticate/user' ).

        lo_client->get_http_request( )->set_header_field(
          i_name  = 'Content-Type'
          i_value = 'application/json' ).

        lo_client->get_http_request( )->set_text( lv_auth_json ).

        lo_response = lo_client->execute( if_web_http_client=>post ).

        DATA(ls_status) = lo_response->get_status( ).

        IF ls_status-code <> 200.
          ev_error = |BMS auth failed — HTTP { ls_status-code }: | &&
                     lo_response->get_text( ).
          lo_client->close( ).
          RETURN.
        ENDIF.

        DATA(lv_body) = lo_response->get_text( ).
        lo_client->close( ).

      CATCH cx_web_http_client_error INTO DATA(lx_err).
        ev_error = |BMS auth HTTP error: { lx_err->get_text( ) }|.
        RETURN.
    ENDTRY.

    TYPES: BEGIN OF ty_auth_response,
             token TYPE string,
           END OF ty_auth_response.
    DATA ls_auth TYPE ty_auth_response.

    /ui2/cl_json=>deserialize(
      EXPORTING json = lv_body
      CHANGING  data = ls_auth ).

    IF ls_auth-token IS INITIAL.
      ev_error = |BMS auth returned no token. Response: { lv_body }|.
      RETURN.
    ENDIF.

    ev_token = |Bearer { ls_auth-token }|.

  ENDMETHOD.

  METHOD post_bms_order.

    CLEAR: ev_http_status, ev_response.

    DATA lo_client   TYPE REF TO if_web_http_client.
    DATA lo_response TYPE REF TO if_web_http_response.

    TRY.
        lo_client = cl_web_http_client_manager=>create_by_url(
                      i_url = iv_base_url &&
                              '/api/container/create-order-halle' ).

        lo_client->get_http_request( )->set_header_field(
          i_name  = 'Content-Type'
          i_value = 'application/json-patch+json' ).

        lo_client->get_http_request( )->set_header_field(
          i_name  = 'Authorization'
          i_value = iv_bearer_token ).

        lo_client->get_http_request( )->set_text( iv_json ).

        lo_response = lo_client->execute( if_web_http_client=>post ).

        DATA(ls_status) = lo_response->get_status( ).
        ev_http_status  = ls_status-code.
        ev_response     = lo_response->get_text( ).

        lo_client->close( ).

      CATCH cx_web_http_client_error INTO DATA(lx_err).
        ev_http_status = 0.
        ev_response    = |BMS POST HTTP error: { lx_err->get_text( ) }|.
    ENDTRY.

  ENDMETHOD.
