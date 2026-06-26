FUNCTION zwr_bms_log_write.

  DATA ls_log  TYPE zbms_api_log.
  DATA lv_uuid TYPE sysuuid_x16.

  " Generate UUID via standard FM — works in all RFC contexts
  CALL FUNCTION 'SYSTEM_UUID_CREATE'
    IMPORTING
      uuid = lv_uuid.

  ls_log-log_uuid        = lv_uuid.
  ls_log-mandt           = iv_mandt.
  ls_log-created_at      = iv_created_at.
  ls_log-created_by      = iv_created_by.
  ls_log-direction       = iv_direction.
  ls_log-tour_uuid       = iv_tour_uuid.
  ls_log-service_uuid    = iv_service_uuid.
  ls_log-order_number    = iv_order_number.
  ls_log-endpoint        = iv_endpoint.
  ls_log-http_status     = iv_http_status.
  ls_log-request_payload = iv_request.
  ls_log-response_body   = iv_response.

  MODIFY zbms_api_log FROM ls_log.

ENDFUNCTION.
