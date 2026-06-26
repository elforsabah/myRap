@EndUserText.label : 'BMS API Communication Log'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zbms_api_log {
  key mandt          : mandt not null;
  key log_uuid       : sysuuid_x16 not null;
  created_at         : timestampl;
  created_by         : syuname;
  direction          : char10;
  tour_uuid          : /plce/pdtour_uuid;
  service_uuid       : /plce/pdservice_uuid;
  order_number       : aufnr;
  endpoint           : char255;
  http_status        : int4;
  request_payload_len : int4;
  request_payload    : zwrbms_payload;
  response_body_len  : int4;
  response_body      : zwrbms_payload;
}
