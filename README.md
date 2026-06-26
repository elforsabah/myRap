Auftrag & für Service & wurde erfolgreich an BMS übermittelt

Auftrag & für Service & konnte nicht an BMS übermittelt werden (HTTP &): &

"=====================================================================
        " STEP 9 — Evaluate response
        "          BMS returns 200 OR 201 — both are success
        "=====================================================================
        DATA(lv_svc_id_out) = condense( |{ ls_svc-reference_id ALPHA = OUT }| ).

        IF lv_http_status = 200 OR lv_http_status = 201.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '019'
                     severity = if_abap_behv_message=>severity-success
                     v1       = lv_order_number
                     v2       = lv_svc_id_out )
          ) TO reported-tour.
        ELSE.
          lv_tour_has_error = abap_true.

          " Extract the message field from the BMS error JSON if present
          " BMS error responses follow: {"error":{"message":"..."}}
          DATA(lv_err_msg)   = lv_response.
          DATA(lv_msg_start) = find( val = lv_response sub = '"message":"' ).
          IF lv_msg_start >= 0.
            lv_msg_start = lv_msg_start + strlen( '"message":"' ).
            DATA(lv_msg_end) = find( val = lv_response sub = '"' off = lv_msg_start ).
            IF lv_msg_end > lv_msg_start.
              lv_err_msg = substring(
                val = lv_response
                off = lv_msg_start
                len = lv_msg_end - lv_msg_start ).
            ENDIF.
          ENDIF.

          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '020'
                     severity = if_abap_behv_message=>severity-error
                     v1       = lv_order_number
                     v2       = lv_svc_id_out
                     v3       = lv_http_status
                     v4       = lv_err_msg )
          ) TO reported-tour.
        ENDIF.
