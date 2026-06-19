@EndUserText.label: 'BMS connection configuration'
@AbapCatalog.enhancement.category: #NOT_EXTENSIBLE
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #C
@AbapCatalog.dataMaintenance: #ALLOWED
define table ztour_bms_cfg {

  key mandt        : mandt not null;
  key config_id    : abap.char(10) not null;  " e.g. 'DEFAULT'
  bms_endpoint_url : abap.char(255);
  bms_username     : abap.char(80);
  bms_password     : abap.char(80);
  active           : abap.char(1);
  description      : abap.char(80);

}
