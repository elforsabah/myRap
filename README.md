METHOD identifycard.
  DATA lt_success_keys TYPE TABLE FOR ACTION RESULT zi_wr_weighingsession\\WeighingSession~identifycard.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
    " Validate contract existence (adjust table name if not VBAK)
    SELECT SINGLE @abap_true FROM vbak
      WHERE vbeln = @<ls_key>-%param-vbeln
      INTO @DATA(lv_exists).
    IF sy-subrc = 0.
      " ✓ Valid: mark step ok
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY WeighingSession
        UPDATE FIELDS ( step1ok )
        WITH VALUE #( ( %tky = <ls_key>-%tky step1ok = abap_true ) ).

      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %msg = new_message(
                 id = 'ZWR_WEIGHBRIGE_MESS'
                 number = '000'
                 severity = if_abap_behv_message=>severity-success
                 v1 = 'Contract is valid' ) )
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

      " Fail the action to reject the promise
      APPEND VALUE #(
        %tky = <ls_key>-%tky
        %action-identifycard = if_abap_behv=>mk-on
        %fail-cause = if_abap_behv=>cause-unspecific  " Or cause-parameter if available
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
    result = lt_result.
  ENDIF.
ENDMETHOD.
