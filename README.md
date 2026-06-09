" After post_bms_order call — log every call for testing
        zcl_wr_pd_tour_helper=>log_bms_call(
          iv_tour_uuid    = ls_tour-TourUUID
          iv_service_uuid = ls_asgmt-ServiceUUID
          iv_order_number = ls_ewa-smaufnr
          iv_endpoint     = '/api/container/create-order-halle'
          iv_http_status  = lv_http_status
          iv_request      = lv_json
          iv_response     = lv_response ).
