LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      
      DATA(lv_input) = <ls_key>-%param-zz_timeadjustment.

      " Requirement: Allow Integers or Decimals with . or , (e.g. 15 or 15.5 or 15,5)
      " Regex: ^ = start, \d+ = numbers, ([.,]\d+)? = optional group of dot/comma and more numbers, $ = end
      FIND REGEX '^\d+([.,]\d+)?$' IN lv_input.

      IF sy-subrc <> 0.
        " Create Failed Key
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %fail-cause = if_abap_behv=>cause-unspecific ) 
               TO failed-service.

        " Create Error Message
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %msg = new_message_with_text( 
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Invalid format. Use format 15, 15.5 or 15,5' ) 
                        %element = VALUE #( zz_timeadjustment = if_abap_behv=>mk-on ) 
                      ) TO reported-service.
      ENDIF.
    ENDLOOP.
