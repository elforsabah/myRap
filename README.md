***********************************************************************************************************************
* Identification
***********************************************************************************************************************
  METHOD identifycard.
  READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
    ENTITY WeighingSession
    ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_sessions).

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).
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

*          APPEND VALUE #(
*          %tky       = <ls_session>-%tky
*          %msg       = new_message(
*                         id       = 'ZWR_WEIGHBRIGE_MESS'
*                         number   = '000'
*                         severity = if_abap_behv_message=>severity-error
*                         v1       = <ls_session>-%param-vbeln )
**              %element-vbeln = if_abap_behv=>mk-on                                 " <-- bind message to Vbeln
*            ) TO reported-weighingsession.
*
*
*                " Fail the action to reject the promise
*          APPEND VALUE #(
*            %tky                 = <ls_session>-%tky
*            %action-identifycard = if_abap_behv=>mk-on
*            %fail-cause          = if_abap_behv=>cause-readonly
*          ) TO failed-weighingsession.


" Append unbound error message to %other (no %tky or %element)
      APPEND VALUE #(
        %tky       = <ls_session>-%tky
        %msg = new_message(
                       id = 'ZWR_WEIGHBRIGE_MESS'
                       number = '001'  " Adjust to your error message number
                       severity = if_abap_behv_message=>severity-error
                       v1 = <ls_session>-%param-vbeln )
      ) TO reported-weighingsession.

      " Fail the action to reject the promise
      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %action-identifycard = if_abap_behv=>mk-on
        %fail-cause = if_abap_behv=>cause-readonly  " Or use cause-unspecific if more appropriate
      ) TO failed-weighingsession.


   ENDIF.
  ENDLOOP.
ENDMETHOD.
