METHOD precheck_adjusttime.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " Get the value
      DATA(lv_input) = <ls_key>-%param-ZeitInMin. 
      
      " FIX 1: Remove leading/trailing spaces so regex '$' works correctly
      CONDENSE lv_input NO-GAPS.

      " Regex: Matches numbers like 15, 15.5, or 15,5
      FIND REGEX '^\d+([.,]\d+)?$' IN lv_input.

      IF sy-subrc <> 0.
        " Create Failed Key
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific )
               TO failed-service.

        " FIX 2: Remove %element. You cannot refer to extension fields in reported-service.
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text(
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Invalid format. Use format 15, 15.5 or 15,5' )
                        " %element line removed to fix Syntax Error
                      ) TO reported-service.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

