 "=====================================================================
        " STEP 9 — Evaluate response
        "          BMS returns 200 OR 201 — both are success
        "=====================================================================
        IF lv_http_status = 200 OR lv_http_status = 201.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '010'
                     severity = if_abap_behv_message=>severity-success
                     v1       = lv_order_number )
          ) TO reported-tour.
        ELSE.
          lv_tour_has_error = abap_true.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '011'
                     severity = if_abap_behv_message=>severity-error
                     v1       = lv_http_status
                     v2       = lv_response )
          ) TO reported-tour.
        ENDIF.

        " Write per-service BMS status to /plce/tpdsrvcst
        DATA(lv_svc_bms_status) = COND string(
          WHEN lv_http_status = 200 OR lv_http_status = 201
          THEN 'FREIGEGEBEN'
          ELSE 'ERROR' ).


<img width="1398" height="612" alt="image" src="https://github.com/user-attachments/assets/1b7c3768-5cc2-4d50-9be4-d9c5a8dc60ed" />
