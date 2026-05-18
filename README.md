REPORT z_test_bms_http.

"=======================================================================
" Local test program for ZCL_WR_PD_TOUR_HELPER HTTP methods
" Run with F8 in SE38
"
" Test sequence:
"   TEST 1 — Authentication        → GET_BMS_BEARER_TOKEN
"   TEST 0 — Get waste catalogue   → GET /api/container/get-container-service-catalogs
"   TEST 2 — POST order            → POST_BMS_ORDER
"
" Payload based on confirmed working BMS example JSON.
"=======================================================================

CONSTANTS:
  lc_base_url TYPE string
    VALUE 'http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest',
  lc_username TYPE string VALUE 'BmsWebApi',
  lc_password TYPE string VALUE 'uh}O1Nm#&lV+2LeS'.

DATA lo_helper   TYPE REF TO zcl_wr_pd_tour_helper.
DATA lv_token    TYPE string.
DATA lv_error    TYPE string.
DATA lv_status   TYPE i.
DATA lv_response TYPE string.

CREATE OBJECT lo_helper.

"=======================================================================
" TEST 1 — Authentication
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 1: Authentication           ==='.
WRITE: / '========================================'.

lo_helper->get_bms_bearer_token(
  EXPORTING
    iv_base_url = lc_base_url
    iv_username = lc_username
    iv_password = lc_password
  IMPORTING
    ev_token    = lv_token
    ev_error    = lv_error ).

IF lv_error IS NOT INITIAL.
  WRITE: / 'RESULT  : FAILED'.
  WRITE: / 'ERROR   :', lv_error.
  RETURN.
ENDIF.

WRITE: / 'RESULT  : PASSED'.
WRITE: / 'Token length :', strlen( lv_token ).
WRITE: / 'Token prefix :', lv_token(7).
WRITE: / 'Token (50)   :', lv_token(50).
SKIP.

"=======================================================================
" TEST 0 — Fetch BMS waste catalogue
"           Runs after TEST 1 — requires Bearer token.
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 0: Get Service Catalogs     ==='.
WRITE: / '========================================'.

DATA lo_http_cat TYPE REF TO if_http_client.
DATA lv_cat_code TYPE i.
DATA lv_cat_resp TYPE string.

cl_http_client=>create_by_url(
  EXPORTING
    url    = lc_base_url &&
             '/api/container/get-container-service-catalogs'
  IMPORTING
    client = lo_http_cat
  EXCEPTIONS
    OTHERS = 4 ).

IF sy-subrc <> 0.
  WRITE: / 'RESULT  : FAILED — could not create HTTP client for catalogue'.
ELSE.

  lo_http_cat->request->set_method(
    if_http_request=>co_request_method_get ).
  lo_http_cat->request->set_header_field(
    name  = 'Authorization'
    value = lv_token ).

  lo_http_cat->send(    EXCEPTIONS OTHERS = 4 ).
  lo_http_cat->receive( EXCEPTIONS OTHERS = 4 ).

  lo_http_cat->response->get_status( IMPORTING code = lv_cat_code ).
  lv_cat_resp = lo_http_cat->response->get_cdata( ).
  lo_http_cat->close( ).

  WRITE: / 'HTTP Status:', lv_cat_code.

  CASE lv_cat_code.
    WHEN 200.
      WRITE: / 'RESULT  : PASSED — catalogue retrieved'.
    WHEN 401.
      WRITE: / 'RESULT  : FAILED — not authorized (401)'.
    WHEN 0.
      WRITE: / 'RESULT  : FAILED — could not connect'.
    WHEN OTHERS.
      WRITE: / 'RESULT  : FAILED — HTTP', lv_cat_code.
  ENDCASE.

  SKIP.
  WRITE: / '--- Catalogue Response Body ---'.

  DATA lv_cat_offset TYPE i VALUE 0.
  DATA lv_cat_len    TYPE i.
  DATA lv_cat_chunk  TYPE string.
  DATA lv_cat_rem    TYPE i.

  lv_cat_len = strlen( lv_cat_resp ).

  IF lv_cat_len = 0.
    WRITE: / '(empty response)'.
  ELSE.
    WHILE lv_cat_offset < lv_cat_len.
      lv_cat_rem = lv_cat_len - lv_cat_offset.
      IF lv_cat_rem >= 100.
        lv_cat_chunk = lv_cat_resp+lv_cat_offset(100).
      ELSE.
        lv_cat_chunk = lv_cat_resp+lv_cat_offset(lv_cat_rem).
      ENDIF.
      WRITE: / lv_cat_chunk.
      lv_cat_offset = lv_cat_offset + 100.
    ENDWHILE.
  ENDIF.

ENDIF.

SKIP.

"=======================================================================
" TEST 2 — POST order using confirmed working BMS payload structure
"
" Key findings from working example:
"   - orderSheet / orderSheetType / placeOfDelivery are optional
"   - producer / recycler / carrier are optional
"   - garbageKey / garbageName are optional
"   - team / contractRelated / signatureRequired are optional
"   - containers uses containerNumberOld + containerNumberNew
"   - movementType U = Umleeren
"   - location needs street/streetNumber/zipCode/city only
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 2: POST Order               ==='.
WRITE: / '========================================'.

DATA(lv_test_json) =
  `{` &&
  `"status":"OK",` &&
  `"orderNumber":"12373434",` &&
  `"customer":{` &&
    `"number":"9566",` &&
    `"name1":"Mustermann2",` &&
    `"name2":"Max2",` &&
    `"street":"Musterstrasse",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"59558",` &&
    `"city":"Musterstadt"` &&
  `},` &&
  `"location":{` &&
    `"street":"Musterstrasse",` &&
    `"streetNumber":"99",` &&
    `"zipCode":"59558",` &&
    `"city":"Musterstadt"` &&
  `},` &&
  `"estimatedDuration":9,` &&
  `"plannedDate":"2026-05-11T08:40:05.577Z",` &&
  `"executionDate":"2026-05-11T08:40:05.577Z",` &&
  `"positions":[{` &&
    `"sortNumber":0,` &&
    `"itemNumber":"79001202",` &&
    `"itemDescription":"Sand (im Big Bag 90 cm x 90 cm x 90 cm)",` &&
    `"quantity":2,` &&
    `"unit":"unit",` &&
    `"itemPrice":5,` &&
    `"positionType":"P"` &&
  `}],` &&
  `"containers":[{` &&
    `"containerNumberOld":"802487",` &&
    `"containerNumberNew":"802512",` &&
    `"movementType":"U",` &&
    `"containerTypeName":"Absetzcontainer 10 cbm",` &&
    `"containerTypeNumber":"AC 10",` &&
    `"customerOwned":false` &&
  `}]` &&
  `}`.

WRITE: / 'JSON length:', strlen( lv_test_json ).
SKIP.

lo_helper->post_bms_order(
  EXPORTING
    iv_base_url     = lc_base_url
    iv_bearer_token = lv_token
    iv_json         = lv_test_json
  IMPORTING
    ev_http_status  = lv_status
    ev_response     = lv_response ).

WRITE: / 'HTTP Status:', lv_status.

CASE lv_status.
  WHEN 201.
    WRITE: / 'RESULT  : PASSED — order accepted by BMS (201 Created)'.
    WRITE: / 'NEXT    : End-to-end HTTP flow confirmed.'.
    WRITE: / '          Update ty_container in touranBMSfreigeben'.
    WRITE: / '          to include containerNumberOld + containerNumberNew.'.
  WHEN 400.
    WRITE: / 'RESULT  : FAILED — BMS rejected the payload (400 Bad Request)'.
    WRITE: / 'ACTION  : Read response body below for exact field errors.'.
  WHEN 401.
    WRITE: / 'RESULT  : FAILED — not authorized (401 Unauthorized)'.
    WRITE: / 'ACTION  : Re-run — token may have expired.'.
  WHEN 404.
    WRITE: / 'RESULT  : FAILED — endpoint not found (404)'.
    WRITE: / 'ACTION  : Check base URL and endpoint path.'.
  WHEN 500.
    WRITE: / 'RESULT  : FAILED — BMS internal error (500)'.
    WRITE: / 'ACTION  : Read response body below for BMS error detail.'.
  WHEN 0.
    WRITE: / 'RESULT  : FAILED — HTTP client could not connect'.
    WRITE: / 'ACTION  : Check network connectivity.'.
  WHEN OTHERS.
    WRITE: / 'RESULT  : FAILED — unexpected HTTP status'.
ENDCASE.

SKIP.
WRITE: / '--- BMS Response Body ---'.

DATA lv_offset    TYPE i VALUE 0.
DATA lv_len       TYPE i.
DATA lv_chunk     TYPE string.
DATA lv_remaining TYPE i.

lv_len = strlen( lv_response ).

IF lv_len = 0.
  WRITE: / '(empty response body)'.
ELSE.
  WHILE lv_offset < lv_len.
    lv_remaining = lv_len - lv_offset.
    IF lv_remaining >= 100.
      lv_chunk = lv_response+lv_offset(100).
    ELSE.
      lv_chunk = lv_response+lv_offset(lv_remaining).
    ENDIF.
    WRITE: / lv_chunk.
    lv_offset = lv_offset + 100.
  ENDWHILE.
ENDIF.

SKIP.
WRITE: / '--- End of Test ---'.
