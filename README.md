@EndUserText.label: 'Print Slip Result'
define abstract entity ZAE_WR_WEIGHB_PRINT_Result
  
{
    PDFBase64 : abap.string( 0 );
    
}


managed implementation in class zbp_i_wr_weighingsession unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table zwr_weighsession
draft table ZWR_WEIGHB_DD
lock master
total etag Grossweight
authorization master ( instance )
etag master Grossweight

{
//  Keys must be readonly in strict draft
  field (readonly) Sessionid;  // <-- use your exact CDS element names
  field (numbering: managed) Sessionid;
create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIGHINGSESSION result [1] $self ;
action determineWeight parameter ZAE_WR_WEIGHB_DW  result [1] $self;
function printSlip parameter ZAE_WR_WEIGHB_PRINT  result [1] ZAE_WR_WEIGHB_PRINT_Result;


//action identifyCard  result [1] $self;

validation validateStep1 on save { field Vbeln, Sessionid; } // Identification
validation validateStep2 on save { field Loadtype; } // Load type
validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH

determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math


}


CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS identifyCard FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~identifyCard RESULT result.

    METHODS printSlip FOR read
      IMPORTING keys FOR FUNCTION WeighingSession~printSlip RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS determineWeight FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~determineWeight RESULT result.

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
    DATA: lv_base64 TYPE string,
          lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_weighingsession.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Fetch the base64 PDF using the helper method (use parameters if needed for dynamic generation, e.g., <ls_key>-%param-vbeln)
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

        " Collect for result
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
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

    " Read current entity data for successful keys and add the computed pdf_base64 to the result (no modification)
    IF lt_success_keys IS NOT INITIAL.
      READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        ALL FIELDS WITH lt_success_keys
        RESULT DATA(lt_sessions)
        FAILED DATA(lt_read_failed).

      result = VALUE #( FOR <ls_session> IN lt_sessions
                        ( %tky = <ls_session>-%tky
                          sessionid = <ls_session>-sessionid  " Map other fields as needed
                          " ... (add all relevant entity fields here)
                          PDFBase64 = lv_base64 ) ).  " Set the computed Base64
    ENDIF.
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

