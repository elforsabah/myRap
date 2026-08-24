*&---------------------------------------------------------------------*
*& Report ZZ_BMS_LOG_SHOWS
*&---------------------------------------------------------------------*
REPORT zz_bms_log_shows.

PARAMETERS:
  p_svc   TYPE /plce/pdservice_uuid,
  p_tour  TYPE /plce/pdtour_uuid,
  p_rows  TYPE i DEFAULT 10,
  p_errs  AS CHECKBOX DEFAULT 'X'.   " only failed calls

DATA:
  lv_date TYPE d,
  lv_time TYPE t,
  lv_ts   TYPE timestampl,
  lv_tz   TYPE timezone,
  lv_txt  TYPE string.

START-OF-SELECTION.

  lv_tz = sy-zonlo.
  IF lv_tz IS INITIAL.
    lv_tz = 'CET'.          " CONVERT dumps on an empty time zone
  ENDIF.

  SELECT tour_uuid, service_uuid, pobjnr, created_at, created_by,
         direction, order_number, endpoint, http_status,
         request_payload, response_body
    FROM zbms_api_log
    ORDER BY created_at DESCENDING
    INTO TABLE @DATA(lt_log)
    UP TO @p_rows ROWS.

  LOOP AT lt_log INTO DATA(ls).

    IF p_svc  IS NOT INITIAL AND ls-service_uuid <> p_svc.  CONTINUE. ENDIF.
    IF p_tour IS NOT INITIAL AND ls-tour_uuid    <> p_tour. CONTINUE. ENDIF.
    IF p_errs = abap_true
       AND ( ls-http_status = 200 OR ls-http_status = 201 ). CONTINUE. ENDIF.

    CLEAR: lv_date, lv_time.
    lv_ts = ls-created_at.
    CONVERT TIME STAMP lv_ts TIME ZONE lv_tz
      INTO DATE lv_date TIME lv_time.

    ULINE.
    WRITE: / 'Zeitpunkt   :', lv_date DD/MM/YYYY, lv_time.
    WRITE: / 'Benutzer    :', ls-created_by.
    WRITE: / 'Tour        :', ls-tour_uuid.
    WRITE: / 'Service     :', ls-service_uuid.
    WRITE: / 'POBJNR      :', ls-pobjnr.
    WRITE: / 'Auftrag     :', ls-order_number.
    WRITE: / 'Endpoint    :', ls-endpoint.
    WRITE: / 'HTTP Status :', ls-http_status.
    SKIP.

    WRITE: / '--- REQUEST ---'.
    lv_txt = ls-request_payload.
    PERFORM dump USING lv_txt.
    SKIP.

    WRITE: / '--- RESPONSE ---'.
    lv_txt = ls-response_body.
    PERFORM dump USING lv_txt.
    SKIP.

  ENDLOOP.

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
