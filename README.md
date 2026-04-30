"=======================================================================
" TEST 0 — Fetch BMS waste catalogue (valid garbageKey values)
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 0: Get Service Catalogs     ==='.
WRITE: / '========================================'.

DATA lv_cat_status   TYPE i.
DATA lv_cat_response TYPE string.
DATA lo_http_cat     TYPE REF TO if_http_client.

cl_http_client=>create_by_url(
  EXPORTING
    url    = lc_base_url && '/api/container/get-container-service-catalogs'
  IMPORTING
    client = lo_http_cat
  EXCEPTIONS
    OTHERS = 4 ).

IF sy-subrc <> 0.
  WRITE: / 'FAILED — could not create HTTP client for catalogue'.
ELSE.

  lo_http_cat->request->set_method( if_http_request=>co_request_method_get ).
  lo_http_cat->request->set_header_field(
    name  = 'Authorization'
    value = lv_token ).

  lo_http_cat->send(    EXCEPTIONS OTHERS = 4 ).
  lo_http_cat->receive( EXCEPTIONS OTHERS = 4 ).

  DATA lv_cat_code TYPE i.
  lo_http_cat->response->get_status( IMPORTING code = lv_cat_code ).
  lv_cat_response = lo_http_cat->response->get_cdata( ).
  lo_http_cat->close( ).

  WRITE: / 'HTTP Status:', lv_cat_code.

  IF lv_cat_code = 200.
    WRITE: / 'PASSED — catalogue retrieved'.
  ELSE.
    WRITE: / 'FAILED — HTTP', lv_cat_code.
  ENDIF.

  SKIP.
  WRITE: / '--- Catalogue Response ---'.

  " Print in 100-char chunks
  DATA lv_cat_offset TYPE i VALUE 0.
  DATA lv_cat_len    TYPE i.
  DATA lv_cat_chunk  TYPE string.
  DATA lv_cat_rem    TYPE i.

  lv_cat_len = strlen( lv_cat_response ).

  IF lv_cat_len = 0.
    WRITE: / '(empty response)'.
  ELSE.
    WHILE lv_cat_offset < lv_cat_len.
      lv_cat_rem = lv_cat_len - lv_cat_offset.
      IF lv_cat_rem >= 100.
        lv_cat_chunk = lv_cat_response+lv_cat_offset(100).
      ELSE.
        lv_cat_chunk = lv_cat_response+lv_cat_offset(lv_cat_rem).
      ENDIF.
      WRITE: / lv_cat_chunk.
      lv_cat_offset = lv_cat_offset + 100.
    ENDWHILE.
  ENDIF.

ENDIF.

SKIP.
