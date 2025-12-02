           APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'Z_MSG_CL_SERVICE_EXT'
                   number = '003'
                   severity = if_abap_behv_message=>severity-success
                   v1 = lv_start DATE = USER  ) )
          TO reported-%other.
      
      
        INSERT new_message_with_text(
          
           severity = if_abap_behv_message=>severity-error
           text     = |First Tour Date { lv_start DATE = USER } must not be empty.| )
         INTO TABLE reported-%other.
