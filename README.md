IF sy-subrc = 0.

  "Message to ALL action-call entries in this group (safe: no <key> usage)
  LOOP AT GROUP <group> ASSIGNING FIELD-SYMBOL(<kmsg>).
    APPEND VALUE #(
      %cid = <kmsg>-%cid
      %msg = new_message(
               id       = 'Z_MSG_CL_SERVICE_EXT'
               number   = '004'
               v1       = <group>-template
               v2       = |{ lv_current_date DATE = USER }|
               severity = if_abap_behv_message=>severity-warning
             )
    ) TO reported-tour.
  ENDLOOP.

  lv_current_date = lv_current_date + 1.
  CONTINUE. "already exists -> skip

ENDIF.
