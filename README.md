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
