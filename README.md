    %tky       = <ls_session>-%tky
          %msg       = new_message(
                         id       = 'ZWR_WEIGHBRIGE_MESS'
                         number   = '000'
                         severity = if_abap_behv_message=>severity-error
                         v1       = 'Contract is is Invalid' )
              %element-vbeln = if_abap_behv=>mk-on                                 " <-- bind message to Vbeln
            ) TO reported-weighingsession.

                " Fail the action to reject the promise
          APPEND VALUE #(
            %tky                 = <ls_session>-%tky
            %action-identifycard = if_abap_behv=>mk-off
            %fail-cause          = if_abap_behv=>cause-readonly
          ) TO failed-weighingsession.


   ENDIF.
