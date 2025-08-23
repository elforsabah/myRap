" Fail the action to reject the promise
      APPEND VALUE #(
        %tky                 = <ls_session>-%tky
        %action-identifycard = if_abap_behv=>mk-failed
        %fail-cause          = if_abap_behv=>cause-unspecific
      ) TO failed-weighingsession.
