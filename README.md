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
" TEST 0 runs after TEST 1 because it needs the Bearer token.
" Use the garbageKey from TEST 0 output in the TEST 2 JSON.
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
"           Must run after TEST 1 — requires Bearer token.
"           Use a garbageKey value from the response in TEST 2.
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 0: Get Service Catalogs     ==='.
WRITE: / '========================================'.

DATA lo_http_cat   TYPE REF TO if_http_client.
DATA lv_cat_code   TYPE i.
DATA lv_cat_resp   TYPE string.

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
      WRITE: / 'ACTION  : Find a valid garbageKey below and copy it'.
      WRITE: / '          into the garbageKey field in TEST 2 JSON.'.
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
" TEST 2 — POST a valid order using the token from TEST 1
"
" !! BEFORE RUNNING !!
" Replace garbageKey and garbageName below with real values
" from the TEST 0 catalogue output above.
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 2: POST Order               ==='.
WRITE: / '========================================'.

DATA(lv_test_json) =
  `{"status":"OK",` &&
  `"orderNumber":"TEST-001",` &&
  `"orderSheet":"LS-2026-001",` &&
  `"orderSheetType":"LS",` &&
  `"customer":{` &&
    `"number":"0000001234",` &&
    `"name1":"Testkunde GmbH",` &&
    `"name2":"",` &&
    `"street":"Teststrasse",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"06112",` &&
    `"city":"Halle"` &&
  `},` &&
  `"placeOfDelivery":{` &&
    `"number":"0000001234",` &&
    `"name1":"Testkunde GmbH",` &&
    `"name2":"",` &&
    `"street":"Lieferstrasse",` &&
    `"streetNumber":"5",` &&
    `"zipCode":"06112",` &&
    `"city":"Halle"` &&
  `},` &&
  `"location":{` &&
    `"number":"",` &&
    `"name1":"",` &&
    `"name2":"",` &&
    `"street":"Standplatz Nord",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"06112",` &&
    `"city":"Halle"` &&
  `},` &&
  `"estimatedDuration":30,` &&
  `"plannedDate":"2026-05-01T00:00:00.000Z",` &&
  `"executionDate":"2026-05-01T00:00:00.000Z",` &&
  `"executionTimeFrameStart":"08:00",` &&
  `"executionTimeFrameEnd":"12:00",` &&
  `"executionTime":"08:00 bis 12:00",` &&
  `"notes":"Testauftrag vom SAP",` &&
  `"specialNotes":"",` &&
  `"producer":{` &&
    `"number":"0000005001",` &&
    `"name1":"Test Erzeuger GmbH",` &&
    `"name2":"",` &&
    `"street":"Erzeugerstrasse",` &&
    `"streetNumber":"10",` &&
    `"zipCode":"06110",` &&
    `"city":"Halle"` &&
  `},` &&
  `"recycler":{` &&
    `"number":"0000006001",` &&
    `"name1":"Test Entsorger GmbH",` &&
    `"name2":"",` &&
    `"street":"Entsorgerstrasse",` &&
    `"streetNumber":"20",` &&
    `"zipCode":"06108",` &&
    `"city":"Halle"` &&
  `},` &&
  `"carrier":{` &&
    `"number":"0000007001",` &&
    `"name1":"Test Befoerderer GmbH",` &&
    `"name2":"",` &&
    `"street":"Befoerdererstrasse",` &&
    `"streetNumber":"30",` &&
    `"zipCode":"06106",` &&
    `"city":"Halle"` &&
  `},` &&
  `"garbageKey":"MAT-TEST-001",` &&
  `"garbageName":"Testmaterial Beschreibung",` &&
  `"collectiveConsignmentNoteNumber":"",` &&
  `"team":"Kolonne Test",` &&
  `"contractRelated":false,` &&
  `"signatureRequired":false,` &&
  `"positions":[{` &&
    `"sortNumber":1,` &&
    `"itemNumber":"10000001",` &&
    `"itemDescription":"Containerservice",` &&
    `"quantity":1.0,` &&
    `"unit":"ST",` &&
    `"itemPrice":100.00,` &&
    `"positionType":"P"` &&
  `}],` &&
  `"containers":[{` &&
    `"quantity":1,` &&
    `"movementType":"S",` &&
    `"containerTypeName":"Container 10 cbm",` &&
    `"containerTypeNumber":"C10",` &&
    `"containerNumberNew":"C10-TEST-001",` &&
    `"customerOwned":false` &&
  `}]}`.

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
    WRITE: / 'ACTION  : Check garbageKey value from TEST 0 catalogue.'.
    WRITE: / '          Replace MAT-TEST-001 with a real key.'.
  WHEN 0.
    WRITE: / 'RESULT  : FAILED — HTTP client could not connect'.
    WRITE: / 'ACTION  : Check network connectivity and SM59.'.
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
