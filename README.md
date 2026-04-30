CLASS ltc_bms_http_test DEFINITION FOR TESTING
  DURATION LONG
  RISK LEVEL DANGEROUS.

  PRIVATE SECTION.

    DATA mo_helper TYPE REF TO zcl_wr_pd_tour_helper.

    CONSTANTS:
      lc_base_url TYPE string
        VALUE 'http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest',
      lc_username  TYPE string VALUE 'BmsWebApi',
      lc_password  TYPE string VALUE 'uh}O1Nm#&lV+2LeS'.

    METHODS setup.
    METHODS test_get_bearer_token      FOR TESTING.
    METHODS test_post_order_valid      FOR TESTING.
    METHODS test_post_order_no_token   FOR TESTING.
    METHODS test_post_order_bad_url    FOR TESTING.

ENDCLASS.

CLASS ltc_bms_http_test IMPLEMENTATION.

  METHOD setup.
    CREATE OBJECT mo_helper.
  ENDMETHOD.

  METHOD test_get_bearer_token.
    "-------------------------------------------------------------------
    " Happy path: valid credentials → token returned, no error
    "-------------------------------------------------------------------
    DATA lv_token TYPE string.
    DATA lv_error TYPE string.

    mo_helper->get_bms_bearer_token(
      EXPORTING
        iv_base_url = lc_base_url
        iv_username = lc_username
        iv_password = lc_password
      IMPORTING
        ev_token    = lv_token
        ev_error    = lv_error ).

    cl_abap_unit_assert=>assert_initial(
      act = lv_error
      msg = |Auth should succeed but got error: { lv_error }| ).

    cl_abap_unit_assert=>assert_not_initial(
      act = lv_token
      msg = 'Token must not be empty on success' ).

    " Token must start with 'Bearer '
    cl_abap_unit_assert=>assert_true(
      act = xsdbool( lv_token(7) = 'Bearer ' )
      msg = |Token must start with "Bearer " but got: { lv_token(20) }| ).

    " Token length should be ~380 chars
    DATA(lv_len) = strlen( lv_token ).
    cl_abap_unit_assert=>assert_true(
      act = xsdbool( lv_len > 350 AND lv_len < 420 )
      msg = |Token length { lv_len } is outside expected range 350-420| ).

  ENDMETHOD.

  METHOD test_post_order_valid.
    "-------------------------------------------------------------------
    " Happy path: get a real token then POST a minimal valid order
    " BMS should return 201 Created
    "-------------------------------------------------------------------

    " Step 1: get token
    DATA lv_token TYPE string.
    DATA lv_error TYPE string.

    mo_helper->get_bms_bearer_token(
      EXPORTING
        iv_base_url = lc_base_url
        iv_username = lc_username
        iv_password = lc_password
      IMPORTING
        ev_token    = lv_token
        ev_error    = lv_error ).

    cl_abap_unit_assert=>assert_initial(
      act = lv_error
      msg = |Auth failed before POST test: { lv_error }| ).

    " Step 2: build minimal valid JSON matching BMS Swagger schema
    DATA(lv_json) =
      `{` &&
      `"status":"OK",` &&
      `"orderNumber":"TEST-UNIT-001",` &&
      `"orderSheet":"",` &&
      `"orderSheetType":"LS",` &&
      `"customer":{` &&
      `  "number":"0000100001",` &&
      `  "name1":"Testfirma GmbH","name2":"",` &&
      `  "street":"Teststraße","streetNumber":"1",` &&
      `  "zipCode":"06112","city":"Halle"` &&
      `},` &&
      `"placeOfDelivery":{` &&
      `  "number":"","name1":"","name2":"",` &&
      `  "street":"","streetNumber":"","zipCode":"","city":""` &&
      `},` &&
      `"location":{` &&
      `  "number":"","name1":"","name2":"",` &&
      `  "street":"Standplatz Nord","streetNumber":"",` &&
      `  "zipCode":"","city":""` &&
      `},` &&
      `"estimatedDuration":30,` &&
      `"plannedDate":"2026-05-01T00:00:00.000Z",` &&
      `"executionDate":"2026-05-01T00:00:00.000Z",` &&
      `"executionTimeFrameStart":"",` &&
      `"executionTimeFrameEnd":"",` &&
      `"executionTime":"ab 08:00",` &&
      `"notes":"Testauftrag ABAP Unit Test",` &&
      `"specialNotes":"",` &&
      `"producer":{` &&
      `  "number":"","name1":"","name2":"",` &&
      `  "street":"","streetNumber":"","zipCode":"","city":""` &&
      `},` &&
      `"recycler":{` &&
      `  "number":"","name1":"","name2":"",` &&
      `  "street":"","streetNumber":"","zipCode":"","city":""` &&
      `},` &&
      `"carrier":{` &&
      `  "number":"","name1":"","name2":"",` &&
      `  "street":"","streetNumber":"","zipCode":"","city":""` &&
      `},` &&
      `"garbageKey":"MAT-TEST-001",` &&
      `"garbageName":"Testmaterial",` &&
      `"collectiveConsignmentNoteNumber":"",` &&
      `"team":"Kolonne Test",` &&
      `"contractRelated":false,` &&
      `"signatureRequired":false,` &&
      `"positions":[],` &&
      `"containers":[{` &&
      `  "quantity":1,` &&
      `  "movementType":"S",` &&
      `  "containerTypeName":"Container 10 cbm",` &&
      `  "containerTypeNumber":"C10",` &&
      `  "customerOwned":false` &&
      `}]` &&
      `}`.

    " Step 3: POST
    DATA lv_status   TYPE i.
    DATA lv_response TYPE string.

    mo_helper->post_bms_order(
      EXPORTING
        iv_base_url     = lc_base_url
        iv_bearer_token = lv_token
        iv_json         = lv_json
      IMPORTING
        ev_http_status  = lv_status
        ev_response     = lv_response ).

    cl_abap_unit_assert=>assert_equals(
      act = lv_status
      exp = 201
      msg = |BMS POST expected 201 but got { lv_status }. | &&
            |Response: { lv_response }| ).

  ENDMETHOD.

  METHOD test_post_order_no_token.
    "-------------------------------------------------------------------
    " Negative test: POST without a token → BMS should return 401
    "-------------------------------------------------------------------
    DATA lv_status   TYPE i.
    DATA lv_response TYPE string.

    mo_helper->post_bms_order(
      EXPORTING
        iv_base_url     = lc_base_url
        iv_bearer_token = ''           " deliberately empty
        iv_json         = '{"status":"OK","orderNumber":"TEST-NO-TOKEN"}'
      IMPORTING
        ev_http_status  = lv_status
        ev_response     = lv_response ).

    cl_abap_unit_assert=>assert_true(
      act = xsdbool( lv_status = 401 OR lv_status = 403 )
      msg = |Expected 401/403 without token but got { lv_status }| ).

  ENDMETHOD.

  METHOD test_post_order_bad_url.
    "-------------------------------------------------------------------
    " Negative test: unreachable URL → status 0, error message returned
    "-------------------------------------------------------------------
    DATA lv_status   TYPE i.
    DATA lv_response TYPE string.

    mo_helper->post_bms_order(
      EXPORTING
        iv_base_url     = 'http://does-not-exist-bms-server.invalid'
        iv_bearer_token = 'Bearer faketoken'
        iv_json         = '{}'
      IMPORTING
        ev_http_status  = lv_status
        ev_response     = lv_response ).

    cl_abap_unit_assert=>assert_equals(
      act = lv_status
      exp = 0
      msg = |Expected 0 for unreachable host but got { lv_status }| ).

    cl_abap_unit_assert=>assert_not_initial(
      act = lv_response
      msg = 'Error message must be populated when host is unreachable' ).

  ENDMETHOD.

ENDCLASS.
