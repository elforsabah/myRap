FUNCTION zwr_bms_log_write.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  IMPORTING
*"     VALUE(IV_MANDT)        TYPE  MANDT
*"     VALUE(IV_CREATED_AT)   TYPE  TIMESTAMPL
*"     VALUE(IV_CREATED_BY)   TYPE  SYUNAME
*"     VALUE(IV_DIRECTION)    TYPE  CHAR10
*"     VALUE(IV_TOUR_UUID)    TYPE  /PLCE/PDTOUR_UUID
*"     VALUE(IV_SERVICE_UUID) TYPE  /PLCE/PDSERVICE_UUID
*"     VALUE(IV_ORDER_NUMBER) TYPE  AUFNR
*"     VALUE(IV_ENDPOINT)     TYPE  STRING
*"     VALUE(IV_HTTP_STATUS)  TYPE  I
*"     VALUE(IV_REQUEST)      TYPE  STRING
*"     VALUE(IV_RESPONSE)     TYPE  STRING
*"----------------------------------------------------------------------

  DATA ls_log TYPE zbms_api_log.

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

  INSERT zbms_api_log FROM ls_log.

ENDFUNCTION.
