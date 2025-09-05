DATA:
      lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_weighingsession,
      lv_fm_name      TYPE rs38l_fnam,
      ls_docparams    TYPE sfpdocparams,
      ls_outputparams TYPE sfpoutputparams,
      ls_formoutput   TYPE fpformoutput,
      ls_form_data    TYPE STRUCTURE FOR YOUR_FORM_INTERFACE,  " Define a structure matching your Adobe Form interface input, e.g., with Vbeln, Loadtype, Weight, WeighUnit
      lv_pdf_xstring  TYPE xstring,
      lv_base64       TYPE string.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Read the current session data if needed (optional, since params are provided)
      READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        ALL FIELDS WITH VALUE #( ( %tky = <ls_key>-%tky ) )
        RESULT DATA(lt_sessions)
        FAILED DATA(lt_failed).

      IF lt_sessions IS NOT INITIAL.
        DATA(ls_session) = lt_sessions[ 1 ].

        " Prepare form data from parameters
        ls_form_data-vbeln = <ls_key>-%param-vbeln.
        ls_form_data-loadtype = <ls_key>-%param-loadtype.
        ls_form_data-weight = <ls_key>-%param-weight.
        ls_form_data-weighunit = <ls_key>-%param-weighunit.
        " Add more data if needed from ls_session or other sources

        " Set output parameters for PDF generation
        ls_outputparams-nodialog = 'X'.
        ls_outputparams-getpdf = 'X'.
        ls_outputparams-dest = 'LP01'.  " Printer device; adjust as needed
        ls_outputparams-reqimm = 'X'.   " Immediate output

        " Open PDF job
        CALL FUNCTION 'FP_JOB_OPEN'
          CHANGING
            ie_outputparams = ls_outputparams
          EXCEPTIONS
            cancel          = 1
            usage_error     = 2
            system_error    = 3
            internal_error  = 4
            OTHERS          = 5.
        IF sy-subrc <> 0.
          " Handle error
          APPEND VALUE #(
            %tky = <ls_key>-%tky
            %msg = new_message(
                     id = 'ZWR_WEIGHBRIGE_MESS'
                     number = '004'
                     severity = if_abap_behv_message=>severity-error
                     v1 = 'Failed to open PDF job' )
          ) TO reported-weighingsession.
          APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-weighingsession.
          CONTINUE.
        ENDIF.

        " Get function module name for the Adobe Form (replace 'ZWR_WEIGH_SLIP_FORM' with your actual form name)
        TRY.
            CALL FUNCTION 'FP_FUNCTION_MODULE_NAME'
              EXPORTING
                i_name     = 'ZWR_WEIGH_SLIP_FORM'
              IMPORTING
                e_funcname = lv_fm_name.
          CATCH cx_fp_api_repository cx_fp_api_usage cx_fp_api_internal.
            " Handle error
            APPEND VALUE #(
              %tky = <ls_key>-%tky
              %msg = new_message(
                       id = 'ZWR_WEIGHBRIGE_MESS'
                       number = '005'
                       severity = if_abap_behv_message=>severity-error
                       v1 = 'Form not found' )
            ) TO reported-weighingsession.
            APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-weighingsession.
            CALL FUNCTION 'FP_JOB_CLOSE'.
            CONTINUE.
          ENDTRY.

        " Set doc params (e.g., language)
        ls_docparams-langu = sy-langu.
        ls_docparams-country = sy-land.
        ls_docparams-fillable = 'X'.  " If interactive, adjust as needed

        " Call the form function module
        CALL FUNCTION lv_fm_name
          EXPORTING
            /1bcdwb/docparams = ls_docparams
            is_input          = ls_form_data  " Your input structure
          IMPORTING
            /1bcdwb/formoutput = ls_formoutput
          EXCEPTIONS
            usage_error       = 1
            system_error      = 2
            internal_error    = 3
            OTHERS            = 4.
        IF sy-subrc <> 0.
          " Handle error
          APPEND VALUE #(
            %tky = <ls_key>-%tky
            %msg = new_message(
                     id = 'ZWR_WEIGHBRIGE_MESS'
                     number = '006'
                     severity = if_abap_behv_message=>severity-error
                     v1 = 'Form processing failed' )
          ) TO reported-weighingsession.
          APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-weighingsession.
          CALL FUNCTION 'FP_JOB_CLOSE'.
          CONTINUE.
        ENDIF.

        " Close PDF job
        CALL FUNCTION 'FP_JOB_CLOSE'
          EXCEPTIONS
            usage_error    = 1
            system_error   = 2
            internal_error = 3
            OTHERS         = 4.
        IF sy-subrc <> 0.
          " Handle error (optional, as PDF might still be generated)
        ENDIF.

        " Get PDF xstring
        lv_pdf_xstring = ls_formoutput-pdf.

        " Convert to Base64
        lv_base64 = cl_http_utility=>encode_x_base64( lv_pdf_xstring ).

        " Report success
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '007'
                   severity = if_abap_behv_message=>severity-success
                   v1 = 'Print slip generated' )
        ) TO reported-weighingsession.

        " Collect for result (assuming result is $self; if changed to abstract, adjust)
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
      ELSE.
        " Report error if session not found
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '003'
                   severity = if_abap_behv_message=>severity-error
                   v1 = 'Session not found' )
        ) TO reported-weighingsession.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-not_found ) TO failed-weighingsession.
      ENDIF.
    ENDLOOP.

    " For returning Base64, since result is $self, you need to update the entity with a new field, e.g., PdfBase64
    " First, add PdfBase64 : abap.string(0); to ZI_WR_WEIGHINGSESSION CDS view and persistent table
    " Then, in behavior: field (features : instance) PdfBase64;
    " In this method, after generating:
    " MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
    "   ENTITY WeighingSession
    "   UPDATE FIELDS ( PdfBase64 )
    "   WITH VALUE #( ( %tky = <ls_key>-%tky PdfBase64 = lv_base64 ) )
    "   FAILED lt_update_failed
    "   REPORTED lt_reported.

    " Read and return full data for successful instances
    IF lt_success_keys IS NOT INITIAL.
      READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        ALL FIELDS WITH lt_success_keys
        RESULT DATA(lt_result)
        FAILED DATA(lt_read_failed).
      result = CORRESPONDING #( lt_result ).
    ENDIF.
  ENDMETHOD.
