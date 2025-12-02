INSERT VALUE #(
  %msg = new_message(
           id       = 'Z_MSG_CL_SERVICE_EXT'
           number   = '003'
           severity = if_abap_behv_message=>severity-error
           " Format the date in a string template:
           v1       = |{ lv_start DATE = USER }|
         )
) INTO TABLE reported-%other.
