      IF sy-subrc = 0.

        APPEND VALUE #(
             %cid = <key>-%cid
             %msg = new_message(
                      id       = 'Z_MSG_CL_SERVICE_EXT'
                      number   = '004'
*                      v1 =  <group>-template
*                      v2 =  |{ lv_current_date DATE = USER }|
                      severity = if_abap_behv_message=>severity-warning
                    )
            )
            TO reported-tour.
        lv_current_date = lv_current_date + 1.
        CONTINUE. "already exists -> skip
      ENDIF.
