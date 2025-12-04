CLASS lhc_Weighingbridge DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    DATA gs_ls_weighing_result TYPE zwr_swaa_selfweigh_result.
    DATA gv_weigh_transaction_started TYPE abap_bool.
    data gv_ewawa_korsel type ewawa_weightext.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR Weighingbridge RESULT result.

    METHODS printSlip FOR READ
      IMPORTING keys FOR FUNCTION Weighingbridge~printSlip RESULT result.

    METHODS determineWeight FOR MODIFY
      IMPORTING keys FOR ACTION Weighingbridge~determineWeight RESULT result.

    METHODS identifyCard FOR MODIFY
      IMPORTING keys FOR ACTION Weighingbridge~identifyCard RESULT result.

    METHODS identifyKorsel FOR MODIFY
      IMPORTING keys FOR ACTION Weighingbridge~identifyKorsel RESULT result.

ENDCLASS.

CLASS lhc_Weighingbridge IMPLEMENTATION.

******************************************************************************************
* Authorization
******************************************************************************************
  METHOD get_instance_authorizations.
    READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
      ENTITY Weighingbridge
      FIELDS ( SalesDocument ) WITH CORRESPONDING #( keys )
      RESULT DATA(lt_sessions)
      FAILED failed.

    result = VALUE #( FOR ls_session IN lt_sessions
      ( %tky   = ls_session-%tky
*      %create              = if_abap_behv=>auth-allowed
        %update              = if_abap_behv=>auth-allowed
        %action-identifyCard = if_abap_behv=>auth-allowed
        %action-determineWeight  = if_abap_behv=>auth-allowed ) ).
  ENDMETHOD.

******************************************************************************************
* Identification of Card
******************************************************************************************
  METHOD identifyCard.

    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_r_weighbridge.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Validate contract existence (adjust table name if not VBAK)
*      SELECT SINGLE @abap_true FROM zi_wr_r_weighbridge
*        WHERE SalesDocument = @<ls_key>-%param-vbeln

      SELECT SINGLE @abap_true FROM zi_wr_r_weighbridge
        WHERE IDCardNr = @<ls_key>-%param-Vbeln

        INTO @DATA(lv_exists).
      IF sy-subrc = 0.
        " ✓ Valid: report success message only (no update)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '000'
                   severity = if_abap_behv_message=>severity-success
                   v1 = <ls_key>-%param-vbeln ) )
          TO reported-Weighingbridge.

        " Collect for result
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
      ELSE.
        " ✗ Invalid: report bound error and fail the action
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '001'
                   severity = if_abap_behv_message=>severity-error
                   v1 = <ls_key>-%param-vbeln )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind message to Vbeln field
        ) TO reported-Weighingbridge.

        " Fail the function (set failed to prevent result for this key)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-Weighingbridge.
      ENDIF.

      "***************************************************************************
      "Call and check if there is already a Transaction for this Sales Document
      "**************************************************************************
*      ZCL_WEIGHING_HANDLER=>cl_has_open_weighingproc(
*          EXPORTING
*            iv_contractID   = <ls_key>-%param-vbeln                      "Sales Document
*          RECEIVING
*            RV_HAS_WPROC =  DATA(lv_weigh_transaction_started)        "Weighing Transaction: True if there is and false if there is not
*        ).

    ENDLOOP.

*    IF lv_weigh_transaction_started = abap_true .
*      "When there is already a transaction for this
*      APPEND VALUE #(
*        %tky = <ls_key>-%tky
*        %msg = new_message(
*                 id = 'ZWR_WEIGHBRIGE_MESS'
*                 number = '007'
*                 severity = if_abap_behv_message=>severity-success
*                 v1 = <ls_key>-%param-vbeln ) )
*        TO reported-Weighingbridge.
*
*      " Collect for result
*      APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
*    ENDIF.

    " Read and return full data for successful instances
    IF lt_success_keys IS NOT INITIAL.
      READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
        ENTITY Weighingbridge
        ALL FIELDS WITH lt_success_keys
        RESULT DATA(lt_result)
        FAILED DATA(lt_read_failed).
      result = CORRESPONDING #( lt_result ).
    ENDIF.
  ENDMETHOD.


******************************************************************************************
* Identification of Korsel
******************************************************************************************
  METHOD identifykorsel.


    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_r_weighbridge.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Validate contract existence (adjust table name if not VBAK)
*      SELECT SINGLE @abap_true FROM zi_wr_r_weighbridge
*        WHERE Korselsnr = @<ls_key>-%param-Korselsnr
*        INTO @DATA(lv_exists).

      gv_ewawa_korsel = keys[ 1 ]-%param-Korselsnr.

      IF sy-subrc = 0.
        " ✓ Valid: report success message only (no update)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '010'
                   severity = if_abap_behv_message=>severity-success
                   v1 = <ls_key>-%param-Korselsnr ) )
          TO reported-Weighingbridge.

        " Collect for result
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
      ELSE.
        " ✗ Invalid: report bound error and fail the action
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '011'
                   severity = if_abap_behv_message=>severity-error
                   v1 = <ls_key>-%param-Korselsnr )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind message to Vbeln field
        ) TO reported-Weighingbridge.

        " Fail the function (set failed to prevent result for this key)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-Weighingbridge.
      ENDIF.
    ENDLOOP.
    " Read and return full data for successful instances
    IF lt_success_keys IS NOT INITIAL.
      READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
        ENTITY Weighingbridge
        ALL FIELDS WITH lt_success_keys
        RESULT DATA(lt_result)
        FAILED DATA(lt_read_failed).
      result = CORRESPONDING #( lt_result ).
    ENDIF.
  ENDMETHOD.
*********************************************************************************************
* Selection of Load type
*********************************************************************************************
  METHOD determineWeight.
    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_r_weighbridge.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
*
TRY.
        ZCL_WR_WEIGHBRIDGE_HELPER=>cl_weighing(
          EXPORTING
            iv_contractid = <ls_key>-%param-Vbeln " Sales Document
            iv_wasteid = <ls_key>-%param-Loadtype "Waste = MATNR
*           * iv_licence_plate = " License plate number
            iv_simulation = abap_false
            iv_user = SY-UNAME " User Name in User Master Record
            iv_commit = abap_false " No need for explicit commit. Commit is handle by RAP framework
          IMPORTING
            iv_weighing_result = gs_ls_weighing_result " returning information from weighing process
        ).
      CATCH cx_eewa_base INTO DATA(lx_error).
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'  " Use your custom message class
                   number = '012'  " Assume a message number for weighing error
                   severity = if_abap_behv_message=>severity-error
                   v1 = lx_error->get_text( ) )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind to relevant field if needed
        ) TO reported-weighingbridge.
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-weighingbridge.
        CONTINUE.  " Skip further processing for this failed key
      ENDTRY.


      " Read the current session data
      READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
        ENTITY Weighingbridge
        ALL FIELDS WITH VALUE #( ( %tky = <ls_key>-%tky ) )
        RESULT DATA(lt_sessions)
        FAILED DATA(lt_failed).

      IF lt_sessions IS NOT INITIAL.
        DATA(ls_session) = lt_sessions[ 1 ].
        DATA lv_unit TYPE weight_unit.
        " Example: Assume weight is calculated or fetched (e.g., from a device or table)
        IF gs_ls_weighing_result-bruttoweight IS NOT INITIAL.
          DATA(lv_grossweight) = gs_ls_weighing_result-bruttoweight. "
          lv_unit   = gs_ls_weighing_result-weightunit.
        ENDIF.
        IF gs_ls_weighing_result-taraweight IS NOT INITIAL.
          DATA(lv_teraweight)  = gs_ls_weighing_result-taraweight. "
          lv_unit   = gs_ls_weighing_result-weightunit.
        ENDIF.

        lv_unit = 'KG'.
        IF lv_grossweight IS INITIAL.
          " Simulate fetching weight
          lv_grossweight =  5674367." Example value in kg
*        " Update the entity with the weight
        ENDIF.

        IF lv_teraweight IS INITIAL.
          " Simulate fetching weight
*         lv_teraweight = 3998765. " Example value in kg
*        " Update the entity with the weight
        ENDIF.

        IF lv_grossweight IS NOT INITIAL.
          " Report success of Gross Weight
          APPEND VALUE #(
            %tky = <ls_key>-%tky
            %msg = new_message(
                     id = 'ZWR_WEIGHBRIGE_MESS'
                     number = '002'
                     severity = if_abap_behv_message=>severity-success
                     v1 = |{ lv_grossweight }  { lv_unit }|
                      )
          ) TO reported-weighingbridge.

        ENDIF.

        IF lv_teraweight IS NOT INITIAL.
          " Report success of Tera weight
          APPEND VALUE #(
            %tky = <ls_key>-%tky
            %msg = new_message(
                     id = 'ZWR_WEIGHBRIGE_MESS'
                     number = '009'
                     severity = if_abap_behv_message=>severity-success
                     v1 = |{ lv_teraweight  }  { lv_unit }| )
          ) TO reported-weighingbridge.

        ENDIF.
        " Collect for result
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
        ) TO reported-weighingbridge.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-not_found ) TO failed-weighingbridge.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

*********************************************************************************************
* Printing of slip
*********************************************************************************************
  METHOD printslip.
    DATA lv_base64 TYPE string.
    DATA lv_b64 TYPE string.
    DATA lv_b64_v2 TYPE string.
    DATA ls_wa TYPE ewa_wa_weighproc.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " Fetch the base64 PDF using the helper method
      ls_wa-brutto_weight  = gs_ls_weighing_result-bruttoweight.   "
      ls_wa-tare_weight    = gs_ls_weighing_result-taraweight.

      ls_wa-brutto_weight  = 5674367.
      ls_wa-tara_weight    = 3998765.
      ls_wa-brutto_unit    = 'KG'.
      ls_wa-tara_unit      = 'KG'.
      ls_wa-vbeln          = <ls_key>-%param-Vbeln.
      ls_wa-waste          = <ls_key>-%param-Loadtype.

      zcl_wr_weighbridge_helper=>form_to_base64(
        EXPORTING
          iv_form   = 'IS_U_WA_SF_WEIGHINGPROCESS'
          iv_lang   = sy-langu
*         iv_show   = abap_true
*         iv_pdf    = abap_false
*         iv_b64    = abap_false
*         iv_pdfpth =
*         iv_b64pth =
*         iv_prevln = 200
          Iv_WA     = ls_wa
        RECEIVING
          r_base64  = lv_base64    "--> This is a string
      ).


      IF   lv_base64 IS NOT INITIAL.
        " Report success message
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '004'  " Assume a new message for PDF generation success
                   severity = if_abap_behv_message=>severity-success
                   v1 = 'PDF generated successfully' )
        ) TO reported-weighingbridge.

        " Directly add to result (combine %tky with result entity fields)
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %param = VALUE #( pdfbase64 = lv_base64 ) ) TO result.
      ELSE.
        " Report error if base64 generation failed
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '006'
                   severity = if_abap_behv_message=>severity-error
                   v1 = 'Failed to generate PDF' )
        ) TO reported-weighingbridge.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-weighingbridge.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.


ENDCLASS.
