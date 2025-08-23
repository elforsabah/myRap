APPEND VALUE #( 
  %tky       = <ls_session>-%tky
  %msg       = new_message(
                 id       = 'ZWR_WEIGHBRIGE_MESS'
                 number   = '001'
                 severity = if_abap_behv_message=>severity-error
                 v1       = <ls_session>-vbeln )
  %element-vbeln = if_abap_behv=>mk-on    " <-- bind message to Vbeln
) TO reported-weighingsession.
