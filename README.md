REPORT z_test_bms_http.

"=======================================================================
" Local test program for ZCL_WR_PD_TOUR_HELPER HTTP methods
" Run with F8 in SE38
"
" Test sequence:
"   TEST 1 — Authentication        → GET_BMS_BEARER_TOKEN
"   TEST 0 — Get waste catalogue   → GET /api/container/get-container-service-catalogs
"   TEST 2 — POST minimal order    → confirmed working minimal payload
"   TEST 3 — POST full order       → full payload with all fields
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
" HELPER: print response body in 100-char chunks
"=======================================================================
DATA lv_pr_offset TYPE i.
DATA lv_pr_len    TYPE i.
DATA lv_pr_chunk  TYPE string.
DATA lv_pr_rem    TYPE i.

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

  lv_pr_offset = 0.
  lv_pr_len    = strlen( lv_cat_resp ).

  IF lv_pr_len = 0.
    WRITE: / '(empty response)'.
  ELSE.
    WHILE lv_pr_offset < lv_pr_len.
      lv_pr_rem = lv_pr_len - lv_pr_offset.
      IF lv_pr_rem >= 100.
        lv_pr_chunk = lv_cat_resp+lv_pr_offset(100).
      ELSE.
        lv_pr_chunk = lv_cat_resp+lv_pr_offset(lv_pr_rem).
      ENDIF.
      WRITE: / lv_pr_chunk.
      lv_pr_offset = lv_pr_offset + 100.
    ENDWHILE.
  ENDIF.

ENDIF.

SKIP.

"=======================================================================
" TEST 2 — POST minimal order
"           Confirmed working payload — minimal required fields only.
"           movementType U = Umleeren
"           No garbageKey / producer / recycler / carrier needed
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 2: POST Minimal Order       ==='.
WRITE: / '========================================'.

DATA(lv_json_t2) =
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

WRITE: / 'JSON length:', strlen( lv_json_t2 ).
SKIP.

lo_helper->post_bms_order(
  EXPORTING
    iv_base_url     = lc_base_url
    iv_bearer_token = lv_token
    iv_json         = lv_json_t2
  IMPORTING
    ev_http_status  = lv_status
    ev_response     = lv_response ).

WRITE: / 'HTTP Status:', lv_status.

CASE lv_status.
  WHEN 201.
    WRITE: / 'RESULT  : PASSED — minimal order accepted (201 Created)'.
  WHEN 400.
    WRITE: / 'RESULT  : FAILED — 400 Bad Request'.
    WRITE: / 'ACTION  : Read response body for field errors.'.
  WHEN 401.
    WRITE: / 'RESULT  : FAILED — 401 Unauthorized. Re-run.'.
  WHEN 500.
    WRITE: / 'RESULT  : FAILED — 500 BMS internal error'.
    WRITE: / 'ACTION  : Read response body for BMS error detail.'.
  WHEN 0.
    WRITE: / 'RESULT  : FAILED — could not connect'.
  WHEN OTHERS.
    WRITE: / 'RESULT  : FAILED — HTTP', lv_status.
ENDCASE.

SKIP.
WRITE: / '--- TEST 2 Response Body ---'.

lv_pr_offset = 0.
lv_pr_len    = strlen( lv_response ).

IF lv_pr_len = 0.
  WRITE: / '(empty response body)'.
ELSE.
  WHILE lv_pr_offset < lv_pr_len.
    lv_pr_rem = lv_pr_len - lv_pr_offset.
    IF lv_pr_rem >= 100.
      lv_pr_chunk = lv_response+lv_pr_offset(100).
    ELSE.
      lv_pr_chunk = lv_response+lv_pr_offset(lv_pr_rem).
    ENDIF.
    WRITE: / lv_pr_chunk.
    lv_pr_offset = lv_pr_offset + 100.
  ENDWHILE.
ENDIF.

SKIP.

"=======================================================================
" TEST 3 — POST full order
"           Full payload with all fields populated.
"           garbageKey 040222 from confirmed working BMS example.
"           Includes producer / recycler / carrier / team /
"           collectiveConsignmentNoteNumber / signatureRequired etc.
"           Special chars in street names replaced with ss / ae / oe / ue
"           to avoid encoding issues in the HTTP body.
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 3: POST Full Order          ==='.
WRITE: / '========================================'.

DATA(lv_json_t3) =
  `{` &&
  `"status":"OK",` &&
  `"orderNumber":"12345",` &&
  `"orderSheet":"11111",` &&
  `"orderSheetType":"LS",` &&
  `"customer":{` &&
    `"number":"9595",` &&
    `"name1":"Mustermann",` &&
    `"name2":"Max",` &&
    `"street":"Musterstrasse",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"59558",` &&
    `"city":"Musterstadt"` &&
  `},` &&
  `"placeOfDelivery":{` &&
    `"street":"Musterstrasse",` &&
    `"streetNumber":"77",` &&
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
  `"executionTimeFrameStart":"08:40:05",` &&
  `"executionTimeFrameEnd":"09:40:05",` &&
  `"executionTime":"",` &&
  `"notes":"Hinweis1",` &&
  `"specialNotes":"Hinweis2",` &&
  `"producer":{` &&
    `"number":"1234",` &&
    `"name1":"Pro1",` &&
    `"name2":"",` &&
    `"street":"ProStrasse",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"06258",` &&
    `"city":"Kollenbey"` &&
  `},` &&
  `"recycler":{` &&
    `"number":"",` &&
    `"name1":"Re1",` &&
    `"name2":"",` &&
    `"street":"Re",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"06258",` &&
    `"city":"Kollenbey"` &&
  `},` &&
  `"carrier":{` &&
    `"number":"2323",` &&
    `"name1":"Car1",` &&
    `"name2":"",` &&
    `"street":"CarStrasse",` &&
    `"streetNumber":"1",` &&
    `"zipCode":"06258",` &&
    `"city":"Kollenbey"` &&
  `},` &&
  `"garbageKey":"040222",` &&
  `"garbageName":"Abfaelle aus verarbeiteten Textilfasern",` &&
  `"collectiveConsignmentNoteNumber":"1212",` &&
  `"team":"ARK 4 F",` &&
  `"contractRelated":true,` &&
  `"signatureRequired":true,` &&
  `"positions":[{` &&
    `"sortNumber":0,` &&
    `"itemNumber":"20000830",` &&
    `"itemDescription":"Strassenkehricht",` &&
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

WRITE: / 'JSON length:', strlen( lv_json_t3 ).
SKIP.

lo_helper->post_bms_order(
  EXPORTING
    iv_base_url     = lc_base_url
    iv_bearer_token = lv_token
    iv_json         = lv_json_t3
  IMPORTING
    ev_http_status  = lv_status
    ev_response     = lv_response ).

WRITE: / 'HTTP Status:', lv_status.

CASE lv_status.
  WHEN 201.
    WRITE: / 'RESULT  : PASSED — full order accepted (201 Created)'.
    WRITE: / 'NEXT    : Both payloads confirmed working.'.
    WRITE: / '          Update ty_container in touranBMSfreigeben'.
    WRITE: / '          to include containerNumberOld field.'.
    WRITE: / '          Map EWA SERNR_NEW → containerNumberNew.'.
    WRITE: / '          Map EWA SERNR    → containerNumberOld.'.
  WHEN 400.
    WRITE: / 'RESULT  : FAILED — 400 Bad Request'.
    WRITE: / 'ACTION  : Read response body for field errors.'.
  WHEN 401.
    WRITE: / 'RESULT  : FAILED — 401 Unauthorized. Re-run.'.
  WHEN 500.
    WRITE: / 'RESULT  : FAILED — 500 BMS internal error'.
    WRITE: / 'ACTION  : garbageKey 040222 may not exist in this'.
    WRITE: / '          BMS environment. Check catalogue from TEST 0.'.
  WHEN 0.
    WRITE: / 'RESULT  : FAILED — could not connect'.
  WHEN OTHERS.
    WRITE: / 'RESULT  : FAILED — HTTP', lv_status.
ENDCASE.

SKIP.
WRITE: / '--- TEST 3 Response Body ---'.

lv_pr_offset = 0.
lv_pr_len    = strlen( lv_response ).

IF lv_pr_len = 0.
  WRITE: / '(empty response body)'.
ELSE.
  WHILE lv_pr_offset < lv_pr_len.
    lv_pr_rem = lv_pr_len - lv_pr_offset.
    IF lv_pr_rem >= 100.
      lv_pr_chunk = lv_response+lv_pr_offset(100).
    ELSE.
      lv_pr_chunk = lv_response+lv_pr_offset(lv_pr_rem).
    ENDIF.
    WRITE: / lv_pr_chunk.
    lv_pr_offset = lv_pr_offset + 100.
  ENDWHILE.
ENDIF.

SKIP.
WRITE: / '--- End of Test ---'.
