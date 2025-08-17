***********************************************************************************************************************
* Identification
***********************************************************************************************************************
  METHOD identifycard.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
     ENTITY WeighingSession
     ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_sessions).

    LOOP AT lt_sessions ASSIGNING FIELD-SYMBOL(<ls_session>).
      " TODO: look up driver/contract by CardId (from keys[ 1 ]-%param-CardId)
      SELECT SINGLE * FROM zwr_weighsession WHERE vbeln = @<ls_session>-Vbeln INTO @DATA(ls_weighsessions).
      IF sy-subrc = 0.
        <ls_session>-step1ok = abap_true.
      ENDIF.

      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession UPDATE FIELDS ( step1ok ) WITH VALUE #( ( %tky = <ls_session>-%tky step1ok = abap_true ) ).
      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
    ENDLOOP.
  ENDMETHOD.

