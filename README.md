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
"
" NOTE: BMS returns 200 OR 201 for success depending on payload variant.
"       Both are treated as PASSED.
"       Change orderNumber on each run to avoid duplicate order errors.
"=======================================================================

CONSTANTS:
  lc_base_url TYPE string
    VALUE 'http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest',
  lc_username TYPE string VALUE 'BmsWebApi',
  lc_password TYPE string VALUE 'uh}O1Nm#&lV+2LeS'.

"-----------------------------------------------------------------------
" !! INCREMENT THESE ON EVERY TEST RUN to avoid duplicate order errors
" BMS returns 500 "Auftrag existiert bereits" if orderNumber is reused
"-----------------------------------------------------------------------
CONSTANTS:
  lc_order_nr_t2 TYPE string VALUE '12373436',  " increment each run
  lc_order_nr_t3 TYPE string VALUE '12345002'.   " increment each run

DATA lo_helper   TYPE REF TO zcl_wr_pd_tour_helper.
DATA lv_token    TYPE string.
DATA lv_error    TYPE string.
DATA lv_status   TYPE i.
DATA lv_response TYPE string.

" Shared print variables
DATA lv_pr_offset TYPE i.
DATA lv_pr_len    TYPE i.
DATA lv_pr_chunk  TYPE string.
DATA lv_pr_rem    TYPE i.

CREATE OBJECT lo_helper.

"=======================================================================
" FORM: print response body in 100-char chunks
"=======================================================================
DEFINE print_response.
  lv_pr_offset = 0.
  lv_pr_len    = strlen( &1 ).
  IF lv_pr_len = 0.
    WRITE: / '(empty response body)'.
  ELSE.
    WHILE lv_pr_offset < lv_pr_len.
      lv_pr_rem = lv_pr_len - lv_pr_offset.
      IF lv_pr_rem >= 100.
        lv_pr_chunk = &1+lv_pr_offset(100).
      ELSE.
        lv_pr_chunk = &1+lv_pr_offset(lv_pr_rem).
      ENDIF.
      WRITE: / lv_pr_chunk.
      lv_pr_offset = lv_pr_offset + 100.
    ENDWHILE.
  ENDIF.
END-OF-DEFINITION.

"=======================================================================
" FORM: evaluate and print HTTP status
"=======================================================================
DEFINE print_status.
  CASE lv_status.
    WHEN 200.
      WRITE: / 'RESULT  : PASSED — accepted by BMS (200 OK)'.
    WHEN 201.
      WRITE: / 'RESULT  : PASSED — accepted by BMS (201 Created)'.
    WHEN 400.
      WRITE: / 'RESULT  : FAILED — 400 Bad Request'.
      WRITE: / 'ACTION  : Read response body for exact field errors.'.
    WHEN 401.
      WRITE: / 'RESULT  : FAILED — 401 Unauthorized'.
      WRITE: / 'ACTION  : Re-run — token may have expired.'.
    WHEN 404.
      WRITE: / 'RESULT  : FAILED — 404 Not Found'.
      WRITE: / 'ACTION  : Check base URL and endpoint path.'.
    WHEN 500.
      WRITE: / 'RESULT  : FAILED — 500 BMS internal error'.
      WRITE: / 'ACTION  : Read response body for BMS error detail.'.
      WRITE: / '          If "existiert bereits" → increment orderNumber'.
      WRITE: / '          constant at top of report and re-run.'.
    WHEN 0.
      WRITE: / 'RESULT  : FAILED — HTTP client could not connect'.
      WRITE: / 'ACTION  : Check network connectivity.'.
    WHEN OTHERS.
      WRITE: / 'RESULT  : FAILED — unexpected HTTP status', lv_status.
  ENDCASE.
END-OF-DEFINITION.

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
"           Currently returns {"data":[]} in the test environment.
"           Production BMS will have real waste keys here.
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
  WRITE: / 'RESULT  : FAILED — could not create HTTP client'.
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
      IF lv_cat_resp = '{"data":[]}' OR lv_cat_resp = `{"data":[]}`.
        WRITE: / 'NOTE    : Catalogue is empty in this BMS environment.'.
        WRITE: / '          TEST 3 uses garbageKey 040222 which may fail'.
        WRITE: / '          with 500 until BMS catalogue is populated.'.
      ENDIF.
    WHEN 401.
      WRITE: / 'RESULT  : FAILED — not authorized (401)'.
    WHEN 0.
      WRITE: / 'RESULT  : FAILED — could not connect'.
    WHEN OTHERS.
      WRITE: / 'RESULT  : FAILED — HTTP', lv_cat_code.
  ENDCASE.

  SKIP.
  WRITE: / '--- Catalogue Response Body ---'.
  print_response lv_cat_resp.

ENDIF.

SKIP.

"=======================================================================
" TEST 2 — POST minimal order
"           Minimal required fields only — no garbageKey / partners.
"           movementType U (Umleeren) confirmed working in BMS.
"           !! Increment lc_order_nr_t2 on each run !!
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 2: POST Minimal Order       ==='.
WRITE: / '========================================'.
WRITE: / 'Order number:', lc_order_nr_t2.
SKIP.

DATA(lv_json_t2) =
  `{"status":"OK",` &&
  `"orderNumber":"` && lc_order_nr_t2 && `",` &&
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
  `}]}`.

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
print_status.

SKIP.
WRITE: / '--- TEST 2 Response Body ---'.
print_response lv_response.
SKIP.

"=======================================================================
" TEST 3 — POST full order
"           All fields populated including producer / recycler / carrier
"           garbageKey 040222 from confirmed working BMS example.
"           BMS returns 200 OK (not 201) for this payload — PASSED.
"           !! Increment lc_order_nr_t3 on each run !!
"=======================================================================
WRITE: / '========================================'.
WRITE: / '=== TEST 3: POST Full Order          ==='.
WRITE: / '========================================'.
WRITE: / 'Order number:', lc_order_nr_t3.
SKIP.

DATA(lv_json_t3) =
  `{"status":"OK",` &&
  `"orderNumber":"` && lc_order_nr_t3 && `",` &&
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
  `}]}`.

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
print_status.

IF lv_status = 200 OR lv_status = 201.
  WRITE: / ''.
  WRITE: / 'NEXT STEPS:'.
  WRITE: / '  1. Both payloads confirmed working end-to-end.'.
  WRITE: / '  2. Add containerNumberOld to ty_container in'.
  WRITE: / '     touranBMSfreigeben and map from EWA SERNR field.'.
  WRITE: / '  3. Update Step 9 success check to accept 200 OR 201.'.
  WRITE: / '  4. BMS catalogue empty — garbageKey validation will'.
  WRITE: / '     work in production once BMS team populates it.'.
ENDIF.

SKIP.
WRITE: / '--- TEST 3 Response Body ---'.
print_response lv_response.

SKIP.
WRITE: / '--- End of Test ---'.
