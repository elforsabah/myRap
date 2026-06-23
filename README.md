" 6c Movement type — read from ZWRVWAALAPRCTP extension
      "    (replaces hardcoded SWITCH '01'→S '02'→H etc.)
      DATA lv_movement_type TYPE string.

      SELECT SINGLE bms_movement_type
        FROM zwrvwaalaprctp
        WHERE leistungsart = @ls_svc-action    " adjust field name to actual key
        INTO @lv_movement_type.

      IF sy-subrc <> 0 OR lv_movement_type IS INITIAL.
        " Fallback to S (Stellen) if not configured — also warn dispatcher
        lv_movement_type = 'S'.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '013'
                   severity = if_abap_behv_message=>severity-warning
                   v1       = ls_svc-action
                   v2       = ls_svc-reference_id )
        ) TO reported-tour.
      ENDIF.

      " Remove the old SWITCH block and the CA '12345' check entirely
      " — the table lookup covers all cases now
