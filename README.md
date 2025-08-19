CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS Submit FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~Submit RESULT result.

    METHODS calcNet FOR DETERMINE ON MODIFY
      IMPORTING keys FOR WeighingSession~calcNet.

    METHODS validateStep1 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep1.

    METHODS validateStep2 FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~validateStep2.
    METHODS identifycard FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~identifycard RESULT result.

    METHODS setloadType FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~setloadType RESULT result.

     METHODS ValidateLoadType FOR VALIDATE ON SAVE
      IMPORTING keys FOR WeighingSession~ValidateLoadType.

ENDCLASS.

CLASS lhc_WeighingSession IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD NextStep.
  ENDMETHOD.

  METHOD Submit.
  ENDMETHOD.

  METHOD calcNet.
  ENDMETHOD.

  METHOD validateStep1.
  ENDMETHOD.

  METHOD validateStep2.
  ENDMETHOD.

  METHOD ValidateLoadType.
  ENDMETHOD.

***********************************************************************************************************************
* Identification
***********************************************************************************************************************
  METHOD identifycard.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
     ENTITY WeighingSession
     ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_sessions).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).
      " TODO: look up driver/contract by vbeln (from keys[ 1 ]-%param-vbeln)
      SELECT SINGLE * FROM zwr_weighsession WHERE vbeln = @<ls_session>-%param-Vbeln INTO @DATA(ls_weighsessions).


      IF sy-subrc = 0.
*        <ls_session>-step1ok = abap_true.

        APPEND VALUE #(
         %msg = new_message(
           id       = 'ZWR_WEIGHBRIGE_MESS'
           number   = '000'
           severity = if_abap_behv_message=>severity-success

           v1       = 'Contract is Valid'

         )
       ) TO reported-weighingsession.

      ENDIF.

      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession UPDATE FIELDS ( step1ok ) WITH VALUE #( ( %tky = <ls_session>-%tky step1ok = abap_true ) ).
      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
    ENDLOOP.
  ENDMETHOD.

***********************************************************************************************************************
* Selection of Load type
***********************************************************************************************************************
  METHOD setloadType.

*   READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
*      ENTITY WeighingSession FIELDS ( loadtype ) WITH CORRESPONDING #( keys )
*      RESULT DATA(lt_sessions).
*    LOOP AT lt_sessions ASSIGNING FIELD-SYMBOL(<ls_session>).
*      <ls_session>-loadtype = keys[ sy-tabix ]-%param-LoadType.
*      <ls_session>-step2ok  = abap_true.
*      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
*        ENTITY WeighingSession UPDATE FIELDS ( loadtype step2ok )
*        WITH VALUE #( ( %tky = <ls_session>-%tky loadtype = <ls_session>-loadtype step2ok = abap_true ) ).
*      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
*    ENDLOOP.

  ENDMETHOD.
ENDCLASS.
