REPORT z_test_bms_http.

"=======================================================================
" Local test program for ZCL_WR_PD_TOUR_HELPER HTTP methods
" Run with F8 in SE38 / execute in ADT
" Tests:
"   1. Authentication  → GET_BMS_BEARER_TOKEN
"   2. Order POST      → POST_BMS_ORDER (uses token from test 1)
"=======================================================================

CONSTANTS:
  lc_base_url TYPE string
    VALUE 'http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest',
  lc_username TYPE string VALUE 'BmsWebApi',
  lc_password TYPE string VALUE 'uh}O1Nm#&lV+2LeS'.

DATA lo_helper    TYPE REF TO zcl_wr_pd_tour_helper.
DATA lv_token     TYPE string.
DATA lv_error     TYPE string.
DATA lv_status    TYPE i.
DATA lv_response  TYPE string.

CREATE OBJECT lo_helper.

"=======================================================================
" TEST 1 — Authentication
"=======================================================================
WRITE: / '=== TEST 1: Authentication ==='.

lo_helper->get_bms_bearer_token(
  EXPORTING
    iv_base_url = lc_base_url
    iv_username = lc_username
    iv_password = lc_password
  IMPORTING
    ev_token    = lv_token
    ev_error    = lv_error ).

IF lv_error IS NOT INITIAL.
  WRITE: / 'FAILED:', lv_error.
  RETURN.
ENDIF.

WRITE: / 'PASSED'.
WRITE: / 'Token length:', strlen( lv_token ).
WRITE: / 'Token prefix:', lv_token(7).
WRITE: / 'Token (first 50):', lv_token(50).
SKIP.

"=======================================================================
" TEST 2 — POST a minimal valid order using the token from Test 1
"=======================================================================
WRITE: / '=== TEST 2: POST Order ==='.

" Minimal JSON that satisfies the BMS Swagger schema
DATA(lv_test_json) =
  `{` &&
  `"status":"OK",` &&
  `"orderNumber":"TEST-001",` &&
  `"orderSheet":"",` &&
  `"orderSheetType":"LS",` &&
  `"customer":{` &&
  `"number":"0000001234",` &&
  `"name1":"Testkunde GmbH",` &&
  `"name2":"",` &&
  `"street":"Teststraße",` &&
  `"streetNumber":"1",` &&
  `"zipCode":"06112",` &&
  `"city":"Halle"` &&
  `},` &&
  `"placeOfDelivery":{` &&
  `"number":"",` &&
  `"name1":"",` &&
  `"name2":"",` &&
  `"street":"Lieferstraße",` &&
  `"streetNumber":"5",` &&
  `"zipCode":"06112",` &&
  `"city":"Halle"` &&
  `},` &&
  `"location":{` &&
  `"number":"",` &&
  `"name1":"",` &&
  `"name2":"",` &&
  `"street":"Standplatz Nord",` &&
  `"streetNumber":"",` &&
  `"zipCode":"",` &&
  `"city":""` &&
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
  `"number":"",` &&
  `"name1":"",` &&
  `"name2":"",` &&
  `"street":"",` &&
  `"streetNumber":"",` &&
  `"zipCode":"",` &&
  `"city":""` &&
  `},` &&
  `"recycler":{` &&
  `"number":"",` &&
  `"name1":"",` &&
  `"name2":"",` &&
  `"street":"",` &&
  `"streetNumber":"",` &&
  `"zipCode":"",` &&
  `"city":""` &&
  `},` &&
  `"carrier":{` &&
  `"number":"",` &&
  `"name1":"",` &&
  `"name2":"",` &&
  `"street":"",` &&
  `"streetNumber":"",` &&
  `"zipCode":"",` &&
  `"city":""` &&
  `},` &&
  `"garbageKey":"MAT-TEST-001",` &&
  `"garbageName":"Testmaterial Beschreibung",` &&
  `"collectiveConsignmentNoteNumber":"",` &&
  `"team":"Kolonne Test",` &&
  `"contractRelated":false,` &&
  `"signatureRequired":false,` &&
  `"positions":[` &&
  `{` &&
  `"sortNumber":1,` &&
  `"itemNumber":"10000001",` &&
  `"itemDescription":"Containerservice",` &&
  `"quantity":1.0,` &&
  `"unit":"ST",` &&
  `"itemPrice":100.00,` &&
  `"positionType":"P"` &&
  `}` &&
  `],` &&
  `"containers":[` &&
  `{` &&
  `"quantity":1,` &&
  `"movementType":"S",` &&
  `"containerTypeName":"Container 10 cbm",` &&
  `"containerTypeNumber":"C10",` &&
  `"customerOwned":false` &&
  `}` &&
  `]` &&
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
    WRITE: / 'PASSED — order accepted by BMS'.
  WHEN 400.
    WRITE: / 'FAILED — BMS rejected the payload (400 Bad Request)'.
    WRITE: / 'Check the JSON against the Swagger schema.'.
  WHEN 401.
    WRITE: / 'FAILED — authentication rejected (401 Unauthorized)'.
    WRITE: / 'Token may have expired — re-run test 1 and test 2 together.'.
  WHEN 0.
    WRITE: / 'FAILED — HTTP client could not connect'.
  WHEN OTHERS.
    WRITE: / 'FAILED — unexpected HTTP status'.
ENDCASE.

SKIP.
WRITE: / '--- BMS Response Body ---'.

" Print response in chunks of 100 chars to avoid line truncation
DATA lv_offset TYPE i VALUE 0.
DATA lv_len    TYPE i.
DATA lv_chunk  TYPE string.
lv_len = strlen( lv_response ).

WHILE lv_offset < lv_len.
  DATA(lv_remaining) = lv_len - lv_offset.
  IF lv_remaining >= 100.
    lv_chunk = lv_response+lv_offset(100).
  ELSE.
    lv_chunk = lv_response+lv_offset(lv_remaining).
  ENDIF.
  WRITE: / lv_chunk.
  lv_offset = lv_offset + 100.
ENDWHILE.
