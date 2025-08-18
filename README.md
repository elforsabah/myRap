    APPEND VALUE #(
         %msg = new_message(
           id       = '/SIEC/CL_DAMEX_MSG'
           number   = '000'
           severity = if_abap_behv_message=>severity-success

           v1       = lv_response

         )
       ) TO reported-/siec/sd_i_lto_damex.
