METHOD identifycard.
  READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
    ENTITY WeighingSession
    ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_sessions).

  LOOP AT lt_sessions ASSIGNING FIELD-SYMBOL(<ls_session>).

    SELECT SINGLE vbeln FROM vbak
      WHERE vbeln = @<ls_session>-Vbeln
      INTO @DATA(lv_vbeln).

    IF sy-subrc = 0.
      " ✓ Valid: mark step ok
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        UPDATE FIELDS ( step1ok )
        WITH VALUE #( ( %tky = <ls_session>-%tky  step1ok = abap_true ) ).

      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %msg = new_message(
                 id       = 'ZWR_WEIGHBRIGE_MESS'
                 number   = '000'
                 severity = if_abap_behv_message=>severity-success
                 v1       = 'Contract is valid' ) )
        TO reported-weighingsession.

      result = VALUE #( ( %tky = <ls_session>-%tky ) ).

    ELSE.
      " ✗ Invalid: keep step NOT ok and fail the action
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        UPDATE FIELDS ( step1ok )
        WITH VALUE #( ( %tky = <ls_session>-%tky  step1ok = abap_false ) ).

      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %msg = new_message(
                 id       = 'ZWR_WEIGHBRIGE_MESS'
                 number   = '000'
                 severity = if_abap_behv_message=>severity-error
                 v1       = 'Contract is not valid' ) )
        TO reported-weighingsession.

      " IMPORTANT: mark failed so FE treats it as an error
      APPEND VALUE #( %tky = <ls_session>-%tky ) TO failed-weighingsession.
    ENDIF.

  ENDLOOP.
ENDMETHOD.
