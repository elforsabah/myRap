CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS printSlip FOR READ
      IMPORTING keys FOR FUNCTION WeighingSession~printSlip RESULT result.

    METHODS determineWeight FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~determineWeight RESULT result.

    METHODS identifyCard FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~identifyCard RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS Submit FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~Submit RESULT result.

    METHODS calcNet FOR DETERMINE ON MODIFY
      IMPORTING keys FOR WeighingSession~calcNet.

    METHODS ValidateLoadType FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~ValidateLoadType.

    METHODS validateStep1 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep1.

    METHODS validateStep2 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep2.

ENDCLASS.

CLASS lhc_WeighingSession IMPLEMENTATION.

******************************************************************************************
* Authorization
******************************************************************************************
  METHOD get_instance_authorizations.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
      ENTITY WeighingSession
      FIELDS ( sessionid ) WITH CORRESPONDING #( keys )
      RESULT DATA(lt_sessions)
      FAILED failed.

    result = VALUE #( FOR ls_session IN lt_sessions
      ( %tky   = ls_session-%tky
*      %create              = if_abap_behv=>auth-allowed
        %update              = if_abap_behv=>auth-allowed
        %delete              = if_abap_behv=>auth-allowed
        %action-identifyCard = if_abap_behv=>auth-allowed
        %action-determineWeight  = if_abap_behv=>auth-allowed
        %action-NextStep     = if_abap_behv=>auth-allowed
        %action-Submit       = if_abap_behv=>auth-allowed ) ).
  ENDMETHOD.
******************************************************************************************
* Identification
******************************************************************************************
  METHOD identifyCard.

    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_weighingsession.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Validate contract existence (adjust table name if not VBAK)
      SELECT SINGLE @abap_true FROM vbak
        WHERE vbeln = @<ls_key>-%param-vbeln
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
          TO reported-weighingsession.

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
          %element-vbeln = if_abap_behv=>mk-on  " Bind message to Vbeln field
        ) TO reported-weighingsession.

        " Fail the function (set failed to prevent result for this key)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-weighingsession.
      ENDIF.

    ENDLOOP.

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


*********************************************************************************************
* Selection of Load type
*********************************************************************************************
  METHOD determineWeight.
    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_weighingsession.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Read the current session data
      READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        ALL FIELDS WITH VALUE #( ( %tky = <ls_key>-%tky ) )
        RESULT DATA(lt_sessions)
        FAILED DATA(lt_failed).

      IF lt_sessions IS NOT INITIAL.
        DATA(ls_session) = lt_sessions[ 1 ].

        " Call the waarevorgang FUBA to get the weight


        " Example: Assume weight is calculated or fetched (e.g., from a device or table)
        DATA(lv_weight) = ls_session-Grossweight. " Adjust logic as needed
        DATA(lv_unit) = 'KG'.
        IF lv_weight IS INITIAL.
          " Simulate fetching weight (replace with actual logic, e.g., from a device or table)
          lv_weight = 345671. " Example value in kg
*        " Update the entity with the weight
*        MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
*          ENTITY WeighingSession
*          UPDATE FIELDS ( Grossweight )
*          WITH VALUE #( ( %tky = <ls_key>-%tky
*                          Grossweight = lv_weight ) )
*          FAILED DATA(lt_update_failed)
*          REPORTED DATA(lt_reported).
        ENDIF.

        " Report success
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '002'
                   severity = if_abap_behv_message=>severity-success
                   v1 = |{ lv_weight }  { lv_unit }| )
        ) TO reported-weighingsession.

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
        ) TO reported-weighingsession.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-not_found ) TO failed-weighingsession.
      ENDIF.
    ENDLOOP.

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
*********************************************************************************************
* Printing of slip
*********************************************************************************************
  METHOD printslip.
    DATA lv_base64 TYPE string.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Fetch the base64 PDF using the helper method
      " TODO: Make dynamic - pass <ls_key>-%param-vbeln, %param-loadtype, %param-weight, %param-weighunit to helper for form data population
      "       (Enhance zcl_wr_weighbridge_helper=>form_to_base64 to accept a structure with these params and fill the Smart Form dynamically)
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
        RECEIVING
          r_base64  = lv_base64
      ).

      IF lv_base64 IS NOT INITIAL.
        " Report success message
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '004'  " Assume a new message for PDF generation success
                   severity = if_abap_behv_message=>severity-success
                   v1 = 'PDF generated successfully' )
        ) TO reported-weighingsession.

        " Directly add to result (combine %tky with result entity fields)
        APPEND VALUE #( %tky = <ls_key>-%tky
                        PDFBASE64 = lv_base64 ) TO result.
      ELSE.
        " Report error if base64 generation failed
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '006'
                   severity = if_abap_behv_message=>severity-error
                   v1 = 'Failed to generate PDF' )
        ) TO reported-weighingsession.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-weighingsession.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

  METHOD NextStep.
  ENDMETHOD.

  METHOD Submit.
  ENDMETHOD.

  METHOD calcNet.
  ENDMETHOD.

  METHOD ValidateLoadType.
  ENDMETHOD.

  METHOD validateStep1.
  ENDMETHOD.

  METHOD validateStep2.
  ENDMETHOD.

ENDCLASS.

@EndUserText.label: 'Print Slip'
define abstract entity ZAE_WR_WEIGHB_PRINT
  
{
     Vbeln : vbeln_va;
     Loadtype : matnr;
     Weight : abap.char(60);
     WeighUnit: abap.char(3);
}

@EndUserText.label: 'Print Slip Result'
define abstract entity ZAE_WR_WEIGHB_PRINT_Result
  
{
    PDFBASE64 : abap.string( 0 );
    
}

Description	Resource	Path	Location	Type
No component exists with the name "PDFBASE64".	ZBP_I_WR_WEIGHINGSESSION (Local Types)	/S4/.adt/classlib/classes/zbp_i_wr_weighingsession	line 224	ABAP Syntax Check Problem





