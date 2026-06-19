METHOD log_bms_call.

    DATA ls_log TYPE zbms_api_log.
    ls_log-log_uuid        = cl_system_uuid=>create_uuid_x16_static( ).
    GET TIME STAMP FIELD ls_log-created_at.          " correct timestampl fill
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
