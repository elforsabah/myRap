DATA: ls_outparams    TYPE sfpoutputparams,
      ls_docparams    TYPE sfpdocparams,
      lv_fm_name      TYPE rs38l_fnam,
      ls_formoutput   TYPE fpformoutput,
      lv_pdf_xstring  TYPE xstring,
      lv_pdf_base64   TYPE string.

" 1) Request PDF back (no popup)
ls_outparams-nodialog = abap_true.
ls_outparams-getpdf   = abap_true.

" Optional document params (language, etc.)
ls_docparams-langu    = sy-langu.  " e.g. 'E' or 'D'

" 2) Open job
CALL FUNCTION 'FP_JOB_OPEN'
  CHANGING
    ie_outputparams = ls_outparams
  EXCEPTIONS
    cancel          = 1
    usage_error     = 2
    system_error    = 3
    internal_error  = 4.
IF sy-subrc <> 0.
  RAISE EXCEPTION TYPE cx_sy_no_handler.  " <-- handle as needed
ENDIF.

" 3) Resolve the generated FM for your form
CALL FUNCTION 'FP_FUNCTION_MODULE_NAME'
  EXPORTING
    i_name     = 'Z_MY_ADOBE_FORM'        " your SFP form name
  IMPORTING
    e_funcname = lv_fm_name
  EXCEPTIONS
    OTHERS     = 1.
IF sy-subrc <> 0.
  CALL FUNCTION 'FP_JOB_CLOSE'.
  RAISE EXCEPTION TYPE cx_sy_no_handler.
ENDIF.

" (Example) Your form's import data (adapt/remove)
" DATA(is_form_data) = VALUE zmy_form_data( ... ).

" 4) Call the form FM (note the /1BCDWB/* parameters)
CALL FUNCTION lv_fm_name
  EXPORTING
    /1BCDWB/DOCPARAMS  = ls_docparams
    " <---- your form imports go here, for example:
    " is_data             = is_form_data
  IMPORTING
    /1BCDWB/FORMOUTPUT = ls_formoutput
  EXCEPTIONS
    usage_error        = 1
    system_error       = 2
    internal_error     = 3
    OTHERS             = 4.

" Always close the job in an ENSURE/FINALLY in real code
CALL FUNCTION 'FP_JOB_CLOSE'.

IF sy-subrc <> 0.
  RAISE EXCEPTION TYPE cx_sy_no_handler.
ENDIF.

" 5) Get PDF as XSTRING (newer releases) or from SOLIX table (older)
IF ls_formoutput-pdf IS NOT INITIAL.
  lv_pdf_xstring = ls_formoutput-pdf.                 " XSTRING already
ELSEIF ls_formoutput-pdf_table IS NOT INITIAL.
  " Older systems: convert SOLIX table -> XSTRING
  lv_pdf_xstring = cl_bcs_convert=>solix_to_xstring( ls_formoutput-pdf_table ).
ELSE.
  RAISE EXCEPTION TYPE cx_sy_no_handler.  " no PDF returned
ENDIF.

" 6) XSTRING -> Base64
lv_pdf_base64 = cl_http_utility=>if_http_utility~encode_x_base64( lv_pdf_xstring ).

" OPTIONAL: Build a data URL if you want to embed it in HTML
" DATA(lv_data_url) = |data:application/pdf;base64,{ lv_pdf_base64 }|.

" ---> lv_pdf_base64 now holds your PDF as Base64
