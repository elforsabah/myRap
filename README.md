REPORT z_adobe_to_base64.

PARAMETERS:
  p_form   TYPE c LENGTH 30 OBLIGATORY DEFAULT 'Z_MY_ADOBE_FORM'  " SFP form name
           LOWER CASE,
  p_lang   TYPE sylangu DEFAULT sy-langu,
  p_show   AS CHECKBOX DEFAULT 'X' USER-COMMAND act,                     " Show Base64 preview
  p_pdf    AS CHECKBOX,                                                  " Save PDF file
  p_b64    AS CHECKBOX,                                                  " Save Base64 text
  p_pdfpth TYPE string LOWER CASE DEFAULT 'C:\Temp\adobe_form.pdf',
  p_b64pth TYPE string LOWER CASE DEFAULT 'C:\Temp\adobe_form_base64.txt',
  p_prevln TYPE i DEFAULT 200.                                           " Preview chars

START-OF-SELECTION.

  DATA: ls_outparams   TYPE sfpoutputparams,
        ls_docparams   TYPE sfpdocparams,
        lv_fm_name     TYPE rs38l_fnam,
        ls_formoutput  TYPE fpformoutput,
        lv_pdf_xstr    TYPE xstring,
        lv_pdf_b64     TYPE string.

  ls_outparams-nodialog = abap_true.
  ls_outparams-getpdf   = abap_true.
  ls_docparams-langu    = p_lang.

  TRY.
      " Open FP job
      CALL FUNCTION 'FP_JOB_OPEN'
        CHANGING
          ie_outputparams = ls_outparams.

      " Resolve generated FM for form name
      CALL FUNCTION 'FP_FUNCTION_MODULE_NAME'
        EXPORTING
          i_name     = p_form
        IMPORTING
          e_funcname = lv_fm_name.

      " === If your SFP interface needs application data, fill it here ===
      " DATA(ls_app_data) = VALUE zmy_form_ctx( ... ).    " <-- your DDIC type
      " ================================================================

      " Call the form FM
      CALL FUNCTION lv_fm_name
        EXPORTING
          /1BCDWB/DOCPARAMS = ls_docparams
          " is_data            = ls_app_data  " <--- example: your interface import(s)
        IMPORTING
          /1BCDWB/FORMOUTPUT = ls_formoutput
        EXCEPTIONS
          usage_error   = 1
          system_error  = 2
          internal_error= 3
          OTHERS        = 4.
    CATCH cx_root INTO DATA(lx).
      " Ensure job gets closed in FINALLY
      RAISE EXCEPTION lx.
  FINALLY.
      " Always close job
      CALL FUNCTION 'FP_JOB_CLOSE'
        EXCEPTIONS
          OTHERS = 1.
  ENDTRY.

  " Get PDF as XSTRING (newer) or convert from SOLIX (older)
  IF ls_formoutput-pdf IS NOT INITIAL.
    lv_pdf_xstr = ls_formoutput-pdf.
  ELSEIF ls_formoutput-pdf_table IS NOT INITIAL.
    lv_pdf_xstr = cl_bcs_convert=>solix_to_xstring( ls_formoutput-pdf_table ).
  ELSE.
    MESSAGE 'No PDF returned by ADS.' TYPE 'E'.
  ENDIF.

  " XSTRING -> Base64
  lv_pdf_b64 = cl_http_utility=>if_http_utility~encode_x_base64( lv_pdf_xstr ).

  " Optional: display a short preview of Base64
  IF p_show = abap_true.
    DATA(lv_len) = p_prevln.
    IF lv_len <= 0 OR lv_len > strlen( lv_pdf_b64 ).
      lv_len = strlen( lv_pdf_b64 ).
    ENDIF.
    WRITE: / 'Base64 (preview):'.
    ULINE.
    WRITE: / lv_pdf_b64( lv_len ).
    SKIP.
    WRITE: / |Total Base64 length: { strlen( lv_pdf_b64 ) } chars|.
  ENDIF.

  " Optional: save PDF to frontend
  IF p_pdf = abap_true.
    DATA(lt_solix) = cl_bcs_convert=>xstring_to_solix( lv_pdf_xstr ).
    cl_gui_frontend_services=>gui_download(
      EXPORTING
        filename                = p_pdfpth
        filetype                = 'BIN'
      CHANGING
        data_tab                = lt_solix
      EXCEPTIONS
        OTHERS                  = 1 ).
    IF sy-subrc = 0.
      WRITE: / |PDF saved to: { p_pdfpth }|.
    ELSE.
      WRITE: / 'Could not save PDF to frontend.' COLOR COL_NEGATIVE.
    ENDIF.
  ENDIF.

  " Optional: save Base64 text to frontend
  IF p_b64 = abap_true.
    DATA(lt_lines) = VALUE stringtab( ( lv_pdf_b64 ) ).
    cl_gui_frontend_services=>gui_download(
      EXPORTING
        filename                = p_b64pth
        filetype                = 'ASC'
      CHANGING
        data_tab                = lt_lines
      EXCEPTIONS
        OTHERS                  = 1 ).
    IF sy-subrc = 0.
      WRITE: / |Base64 saved to: { p_b64pth }|.
    ELSE.
      WRITE: / 'Could not save Base64 to frontend.' COLOR COL_NEGATIVE.
    ENDIF.
  ENDIF.

  IF p_pdf = abap_false AND p_b64 = abap_false AND p_show = abap_false.
    WRITE: / 'Done. (Nothing selected to show or save.)'.
  ENDIF.
