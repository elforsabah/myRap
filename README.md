*&---------------------------------------------------------------------*
*& Report ZZ_TEST_HTTP1
*&---------------------------------------------------------------------*
*& Test harness for the BMS interface.
*&
*&   TEST 1 — Authentication   → GET_BMS_BEARER_TOKEN via SM59 destination
*&   TEST 2 — Health check     → GET /api/user/healthCheck
*&   TEST 3 — Create order     → POST_BMS_ORDER with an InputOrder payload
*&
*& Note: InternalRemark and ContainerMovementTypeInfo are documented as
*& nullable but are REQUIRED by the running API, and [Required] rejects
*& empty strings — both must carry real content.
*&---------------------------------------------------------------------*
REPORT zz_test_http1.

PARAMETERS:
  p_order  TYPE char20 LOWER CASE DEFAULT 'TEST0001' OBLIGATORY,
  p_date   TYPE d                 DEFAULT sy-datum   OBLIGATORY,
  p_move   TYPE char10 LOWER CASE DEFAULT 'change'   OBLIGATORY,
  p_ctype  TYPE char60 LOWER CASE DEFAULT 'Absetzcontainer 10 cbm' OBLIGATORY,
  p_info   TYPE char60 LOWER CASE,
  p_remark TYPE char60 LOWER CASE DEFAULT 'keine',
  p_cold   TYPE char20 LOWER CASE DEFAULT '802487',
  p_cnew   TYPE char20 LOWER CASE DEFAULT '802512',
  p_tour   TYPE char20 LOWER CASE DEFAULT '1229',
  p_send   AS CHECKBOX            DEFAULT 'X'.

DATA:
  lv_dest   TYPE rfcdest,
  lv_token  TYPE string,
  lv_error  TYPE string,
  lv_status TYPE i,
  lv_resp   TYPE string,
  lv_txt    TYPE string,
  lo_http   TYPE REF TO if_http_client.

START-OF-SELECTION.

*----------------------------------------------------------------------*
* Configuration — no credentials in this program
*----------------------------------------------------------------------*
  SELECT SINGLE bms_destination, bms_username, bms_password, active
    FROM ztour_bms_cfg
    WHERE config_id = 'DEFAULT'
    INTO @DATA(ls_cfg).

  IF sy-subrc <> 0.
    WRITE: / 'ABBRUCH : ZTOUR_BMS_CFG hat keinen Eintrag CONFIG_ID = DEFAULT'.
    RETURN.
  ENDIF.

  IF ls_cfg-bms_destination IS INITIAL.
    WRITE: / 'ABBRUCH : BMS_DESTINATION ist nicht gepflegt'.
    RETURN.
  ENDIF.

  lv_dest = ls_cfg-bms_destination.

  WRITE: / '========================================'.
  WRITE: / '=== Konfiguration                    ==='.
  WRITE: / '========================================'.
  WRITE: / 'Destination :', lv_dest.
  WRITE: / 'Benutzer    :', ls_cfg-bms_username.
  WRITE: / 'Aktiv       :', ls_cfg-active.
  SKIP.

*----------------------------------------------------------------------*
* TEST 1 — Authentication
*----------------------------------------------------------------------*
  WRITE: / '========================================'.
  WRITE: / '=== TEST 1: Authentication           ==='.
  WRITE: / '========================================'.

  zcl_wr_pd_tour_helper=>get_bms_bearer_token(
    EXPORTING
      iv_destination = lv_dest
      iv_username    = CONV string( ls_cfg-bms_username )
      iv_password    = CONV string( ls_cfg-bms_password )
    IMPORTING
      ev_token       = lv_token
      ev_error       = lv_error ).

  IF lv_error IS NOT INITIAL.
    WRITE: / 'RESULT  : FAILED'.
    lv_txt = lv_error.
    PERFORM dump USING lv_txt.
    RETURN.
  ENDIF.

  WRITE: / 'RESULT  : PASSED'.
  WRITE: / 'Länge   :', strlen( lv_token ).
  WRITE: / 'Präfix  :', lv_token(7).
  SKIP.

*----------------------------------------------------------------------*
* TEST 2 — Health check
*----------------------------------------------------------------------*
  WRITE: / '========================================'.
  WRITE: / '=== TEST 2: Health Check             ==='.
  WRITE: / '========================================'.

  cl_http_client=>create_by_destination(
    EXPORTING
      destination              = lv_dest
    IMPORTING
      client                   = lo_http
    EXCEPTIONS
      argument_not_found       = 1
      destination_not_found    = 2
      destination_no_authority = 3
      plugin_not_active        = 4
      internal_error           = 5
      OTHERS                   = 6 ).

  IF sy-subrc <> 0.
    WRITE: / 'RESULT  : FAILED — Destination nicht nutzbar, subrc', sy-subrc.
    WRITE: / '          3 = fehlende S_ICF-Berechtigung'.
  ELSE.

    cl_http_utility=>set_request_uri(
      request = lo_http->request
      uri     = '/BmsApiSapTest/api/user/healthCheck' ).

    lo_http->request->set_method( if_http_request=>co_request_method_get ).
    lo_http->request->set_header_field( name  = 'Authorization'
                                        value = lv_token ).

    lo_http->send(    EXCEPTIONS OTHERS = 4 ).
    lo_http->receive( EXCEPTIONS OTHERS = 4 ).

    lo_http->response->get_status( IMPORTING code = lv_status ).
    lv_resp = lo_http->response->get_cdata( ).
    lo_http->close( ).

    WRITE: / 'HTTP Status:', lv_status.
    IF lv_status = 200.
      WRITE: / 'RESULT  : PASSED'.
    ELSE.
      WRITE: / 'RESULT  : FAILED'.
    ENDIF.
    lv_txt = lv_resp.
    PERFORM dump USING lv_txt.
  ENDIF.

  SKIP.

*----------------------------------------------------------------------*
* TEST 3 — Create order
*----------------------------------------------------------------------*
  WRITE: / '========================================'.
  WRITE: / '=== TEST 3: Create Order             ==='.
  WRITE: / '========================================'.

  DATA(lv_planned) = |{ p_date DATE = ISO }T00:00:00.000Z|.
  DATA(lv_move)    = to_lower( condense( CONV string( p_move ) ) ).

  " containerNumberOld/New per movement type:
  "   new     — old empty, new filled
  "   collect — old filled, new empty
  "   change  — both
  DATA(lv_cold) = condense( CONV string( p_cold ) ).
  DATA(lv_cnew) = condense( CONV string( p_cnew ) ).

  CASE lv_move.
    WHEN 'new'.     CLEAR lv_cold.
    WHEN 'collect'. CLEAR lv_cnew.
    WHEN OTHERS.
  ENDCASE.

  " Required by the running API even though the spec says nullable,
  " and [Required] rejects empty strings — so both need real content.
  DATA(lv_info) = condense( CONV string( p_info ) ).
  IF lv_info IS INITIAL.
    lv_info = SWITCH string( lv_move
                WHEN 'new'     THEN 'Aufstellung'
                WHEN 'collect' THEN 'Einzug'
                ELSE                'Wechsel' ).
  ENDIF.

  DATA(lv_remark) = condense( CONV string( p_remark ) ).
  IF lv_remark IS INITIAL.
    lv_remark = 'keine'.
  ENDIF.

  DATA(lv_cont) =
    `{` &&
      `"containerNumberOld":"`        && lv_cold   && `",` &&
      `"containerNumberNew":"`        && lv_cnew   && `",` &&
      `"movementType":"`              && lv_move   && `",` &&
      `"containerTypeName":"`         && condense( CONV string( p_ctype ) ) && `",` &&
      `"containerMovementTypeInfo":"` && lv_info   && `",` &&
      `"internalRemark":"`            && lv_remark && `",` &&
      `"customerOwned":false` &&
    `}`.

  DATA(lv_test_json) =
    `{` &&
      `"status":"ok",` &&
      `"externalSystemID":"` && condense( CONV string( p_tour ) )  && `",` &&
      `"sortNumber":1,` &&
      `"orderNumber":"`      && condense( CONV string( p_order ) ) && `",` &&
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
      `"plannedDate":"` && lv_planned && `",` &&
      `"executionTimeFrameStart":"08:00:00",` &&
      `"executionTimeFrameEnd":"12:00:00",` &&
      `"notes":"Testauftrag aus SAP",` &&
      `"signatureRequired":false,` &&
      `"containers":[` && lv_cont && `]` &&
    `}`.

  WRITE: / '--- REQUEST ---'.
  lv_txt = lv_test_json.
  PERFORM dump USING lv_txt.
  SKIP.

  IF p_send = abap_false.
    WRITE: / 'Nur Anzeige — Kennzeichen "Senden" ist nicht gesetzt.'.
    RETURN.
  ENDIF.

  CLEAR: lv_status, lv_resp.

  zcl_wr_pd_tour_helper=>post_bms_order(
    EXPORTING
      iv_destination  = lv_dest
      iv_bearer_token = lv_token
      iv_json         = lv_test_json
    IMPORTING
      ev_http_status  = lv_status
      ev_response     = lv_resp ).

  WRITE: / 'HTTP Status:', lv_status.

  CASE lv_status.
    WHEN 200 OR 201.
      WRITE: / 'RESULT  : PASSED — Auftrag von BMS angenommen'.
      WRITE: / 'HINWEIS : data = interne BMS-Auftrags-ID'.
    WHEN 400.
      WRITE: / 'RESULT  : FAILED — Payload abgelehnt (400)'.
      WRITE: / 'AKTION  : Fehlertext unten lesen'.
    WHEN 401.
      WRITE: / 'RESULT  : FAILED — nicht autorisiert (401), Token abgelaufen?'.
    WHEN 404.
      WRITE: / 'RESULT  : FAILED — Endpunkt nicht gefunden (404), Pfad prüfen'.
    WHEN 500.
      WRITE: / 'RESULT  : FAILED — interner BMS-Fehler (500)'.
    WHEN 0.
      WRITE: / 'RESULT  : FAILED — keine Verbindung'.
      WRITE: / '          Dienst-Nr. in SM59 gepflegt?'.
    WHEN OTHERS.
      WRITE: / 'RESULT  : FAILED — unerwarteter Status'.
  ENDCASE.

  SKIP.
  WRITE: / '--- RESPONSE ---'.
  lv_txt = lv_resp.
  PERFORM dump USING lv_txt.

  SKIP.
  WRITE: / '--- Ende ---'.

*&---------------------------------------------------------------------*
FORM dump USING iv_text TYPE string.

  CONSTANTS lc_width TYPE i VALUE 100.

  DATA: lv_len    TYPE i,
        lv_offset TYPE i,
        lv_rest   TYPE i.

  lv_len = strlen( iv_text ).

  IF lv_len = 0.
    WRITE: / '(leer)'.
    RETURN.
  ENDIF.

  WHILE lv_offset < lv_len.
    lv_rest = lv_len - lv_offset.
    IF lv_rest >= lc_width.
      WRITE: / iv_text+lv_offset(lc_width).
    ELSE.
      WRITE: / iv_text+lv_offset(lv_rest).
    ENDIF.
    lv_offset = lv_offset + lc_width.
  ENDWHILE.

ENDFORM.
