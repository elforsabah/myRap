      APPEND VALUE #(
               %cid = <key>-%cid
               %msg = new_message(
                        id       = 'Z_MSG_CL_SERVICE_EXT'
                        number   = '003'
                        severity = if_abap_behv_message=>severity-error
                        v1       = |{ lv_start DATE = USER }|
                      )
             )
        TO reported-createtour.
      lv_failed = abap_true.
