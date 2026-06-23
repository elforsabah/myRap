" Log the inbound call
    DATA lv_empty_uuid TYPE /plce/pdtour_uuid.
    CLEAR lv_empty_uuid.

    zcl_wr_pd_tour_helper=>log_bms_call(
      iv_tour_uuid    = lv_empty_uuid       " ← initial UUID, not empty string
      iv_service_uuid = lv_service_uuid
      iv_order_number = lv_smaufnr
      iv_endpoint     = '/sap/bc/zbms/status_update'
      iv_http_status  = 200
      iv_request      = lv_body
      iv_response     = 'OK'
      iv_direction    = 'INBOUND' ).
