CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS identifyCard FOR Modify
      IMPORTING keys FOR ACTION WeighingSession~identifyCard RESULT result.

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
     LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

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
