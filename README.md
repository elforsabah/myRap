METHOD log_bms_call.

  DATA lv_created_at TYPE timestampl.
  GET TIME STAMP FIELD lv_created_at.

  " CALL FUNCTION IN BACKGROUND TASK is permitted inside RAP action
  " handlers — the FM executes after the LUW commits and does the
  " INSERT on its own independent connection
  CALL FUNCTION 'ZWR_BMS_LOG_WRITE'
    IN BACKGROUND TASK
    EXPORTING
      iv_mandt        = sy-mandt
      iv_created_at   = lv_created_at
      iv_created_by   = sy-uname
      iv_direction    = 'OUTBOUND'
      iv_tour_uuid    = iv_tour_uuid
      iv_service_uuid = iv_service_uuid
      iv_order_number = iv_order_number
      iv_endpoint     = iv_endpoint
      iv_http_status  = iv_http_status
      iv_request      = iv_request
      iv_response     = iv_response.

ENDMETHOD.
