METHOD precheck_adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      
      " 1. Convert to STRING to handle trailing spaces correctly
      DATA(lv_input_str) = CONV string( <ls_key>-%param-ZeitInMin ).
      
      " 2. Remove all spaces (now the string actually shrinks)
      CONDENSE lv_input_str NO-GAPS.

      " 3. Check Regex
      " Using [0-9] is safer than \d in some ABAP versions
      " Using matches( ) is cleaner than FIND REGEX for validation
      IF NOT matches( val = lv_input_str regex = '^[0-9]+([.,][0-9]+)?$' ).

        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific )
               TO failed-service.

        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text(
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Invalid format. Use format 15, 15.5 or 15,5' )
                      ) TO reported-service.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.
