CLASS-METHODS storno_bms_order
  IMPORTING
    iv_base_url      TYPE string
    iv_bearer_token  TYPE string
    iv_order_number  TYPE string
    iv_full_json     TYPE string  " pass empty string if minimal storno confirmed
  EXPORTING
    ev_http_status   TYPE i
    ev_response      TYPE string.
