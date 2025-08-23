" Append unbound error message to %other (no %tky or %element)
      APPEND VALUE #(
        %msg = new_message(
                       id = 'ZWR_WEIGHBRIGE_MESS'
                       number = '001'  " Adjust to your error message number
                       severity = if_abap_behv_message=>severity-error
                       v1 = <ls_session>-%param-vbeln )
      ) TO reported-%other.

      " Fail the action to reject the promise
      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %action-identifycard = if_abap_behv=>mk-on
        %fail-cause = if_abap_behv=>cause-readonly  " Or use cause-unspecific if more appropriate
      ) TO failed-weighingsession.
