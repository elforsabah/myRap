METHOD precheck_adjusttime.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

    " 1. Get Input
    DATA(lv_input_str) TYPE string.
    lv_input_str = <ls_key>-%param-ZeitInMin. 

    " 2. Remove spaces (Crucial for regex)
    CONDENSE lv_input_str NO-GAPS.

    " 3. Validate Format 
    " Regex: ^ = Start, -? = Optional Minus, [0-9]+ = Digits, ([.,]...)? = Optional decimals
    IF NOT matches( val = lv_input_str regex = '^-?[0-9]+([.,][0-9]+)?$' ).

      APPEND VALUE #( %tky = <ls_key>-%tky
                      %fail-cause = if_abap_behv=>cause-unspecific )
             TO failed-service.

      APPEND VALUE #( %tky = <ls_key>-%tky
                      %msg = new_message_with_text(
                               severity = if_abap_behv_message=>severity-error
                               text     = 'Invalid format. Use: 45, -45, 15.5' )
                    ) TO reported-service.
    ENDIF.
  ENDLOOP.

ENDMETHOD.
