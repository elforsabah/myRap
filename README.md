"=====================================================================
        " STEP 9 — Evaluate response
        "          BMS returns 200 OR 201 — both are success
        "=====================================================================
        DATA(lv_svc_id_out) = condense( |{ ls_svc-service_id ALPHA = OUT }| ).
        DATA(lv_tour_id_out) = condense( |{ ls_tour-tourid ALPHA = OUT }| ).

        IF lv_http_status = 200 OR lv_http_status = 201.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '021'
                     severity = if_abap_behv_message=>severity-information
                     v1       = lv_order_number
                     v2       = lv_svc_id_out )
          ) TO reported-tour.
        ELSE.
          lv_tour_has_error = abap_true.

          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '022'
                     severity = if_abap_behv_message=>severity-information
                     v1       = lv_order_number
                     v2       = lv_tour_id_out )
          ) TO reported-tour.
        ENDIF.
