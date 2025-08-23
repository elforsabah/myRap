APPEND VALUE #(
        %msg = new_message(
                       id = 'ZWR_WEIGHBRIGE_MESS'
                       number = '001'  " Adjust to your error message number
                       severity = if_abap_behv_message=>severity-error
                       v1 = <ls_session>-%param-vbeln )
      ) TO reported-%other.
