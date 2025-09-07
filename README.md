DATA(lx_pdf) TYPE xstring.

CALL FUNCTION 'SCMS_BASE64_DECODE_STR'
  EXPORTING
    input  = lv_b64
  IMPORTING
    output = lx_pdf
  EXCEPTIONS
    failed = 1
    OTHERS = 2.

IF sy-subrc <> 0.
  MESSAGE 'Base64 decoding failed' TYPE 'E'.
ENDIF.
