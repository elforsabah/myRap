METHOD log_bms_call.

  DATA lv_created_at  TYPE timestampl.
  DATA lv_log_message TYPE string.

  GET TIME STAMP FIELD lv_created_at.

  " Extract the message field from a JSON error response if present.
  " BMS error responses follow the pattern: {"error":{"message":"..."}}
  " For success responses the full response is logged as-is.
  lv_log_message = iv_response.

  DATA(lv_msg_start) = find( val = iv_response sub = '"message":"' ).
  IF lv_msg_start >= 0.
    lv_msg_start = lv_msg_start + strlen( '"message":"' ).
    DATA(lv_msg_end) = find( val = iv_response sub = '"' off = lv_msg_start ).
    IF lv_msg_end > lv_msg_start.
      lv_log_message = substring(
        val = iv_response
        off = lv_msg_start
        len = lv_msg_end - lv_msg_start ).
    ENDIF.
  ENDIF.

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
      iv_response     = lv_log_message.

ENDMETHOD.
