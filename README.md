METHOD precheck_adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " 1. Convert to STRING to safely handle trailing spaces
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMin ).

      " 2. Remove spaces
      CONDENSE lv_input_str NO-GAPS.

      " 3. Validate Format
      " Regex Explanation:
      " ^           = Start of string
      " -?          = Optional Minus sign (allows negative numbers)
      " [0-9]+      = One or more digits
      " ([.,][0-9]+)? = Optional decimal part (dot or comma followed by digits)
      " $           = End of string

      IF NOT matches( val = lv_input_str regex = '^-?[0-9]+([.,][0-9]+)?$' ).

        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific )
               TO failed-service.

        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text(
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Invalid format. Use format: 15, -15, 15.5 or -15,5' )
                      ) TO reported-service.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.
