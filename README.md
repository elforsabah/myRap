@EndUserText.label: 'BMS API Communication Log'
@AbapCatalog.enhancement.category: #NOT_EXTENSIBLE
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #A
@AbapCatalog.dataMaintenance: #DISPLAY_ONLY
define table zbms_api_log {
  key mandt         : mandt not null;
  key log_uuid      : sysuuid_x16 not null;
  created_at        : timestampl;
  created_by        : syuname;
  direction         : char10;        " OUTBOUND / INBOUND
  tour_uuid         : /plce/pdtour_uuid;
  service_uuid      : /plce/pdservice_uuid;
  order_number      : aufnr;
  endpoint          : string;
  http_status       : int4;
  request_payload   : string;
  response_body     : string;
}


CLASS-METHODS log_bms_call
  IMPORTING
    iv_tour_uuid      TYPE /plce/pdtour_uuid
    iv_service_uuid   TYPE /plce/pdservice_uuid
    iv_order_number   TYPE aufnr
    iv_endpoint       TYPE string
    iv_http_status    TYPE i
    iv_request        TYPE string
    iv_response       TYPE string.





METHOD log_bms_call.
  DATA ls_log TYPE zbms_api_log.

  ls_log-log_uuid         = cl_system_uuid=>create_uuid_x16_static( ).
  ls_log-created_at       = cl_abap_context_info=>get_system_time_stamp( ).
  ls_log-created_by       = sy-uname.
  ls_log-direction        = 'OUTBOUND'.
  ls_log-tour_uuid        = iv_tour_uuid.
  ls_log-service_uuid     = iv_service_uuid.
  ls_log-order_number     = iv_order_number.
  ls_log-endpoint         = iv_endpoint.
  ls_log-http_status      = iv_http_status.
  ls_log-request_payload  = iv_request.
  ls_log-response_body    = iv_response.


" After post_bms_order call — log every call for testing
        zcl_wr_pd_tour_helper=>log_bms_call(
          iv_tour_uuid    = ls_tour-TourUUID
          iv_service_uuid = ls_asgmt-ServiceUUID
          iv_order_number = ls_ewa-smaufnr
          iv_endpoint     = '/api/container/create-order-halle'
          iv_http_status  = lv_http_status
          iv_request      = lv_json
          iv_response     = lv_response ).




          


  

  INSERT zbms_api_log FROM ls_log.
ENDMETHOD.
    
