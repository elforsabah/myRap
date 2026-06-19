@EndUserText.label: 'BMS connection configuration'
@AbapCatalog.enhancement.category: #NOT_EXTENSIBLE
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #C
@AbapCatalog.dataMaintenance: #ALLOWED
define table ztour_bms_cfg {

  key mandt        : mandt not null;
  bms_endpoint_url : abap.char(255);
  bms_username     : abap.char(80);
  bms_password     : abap.char(80);
  active           : abap.char(1);    " X = send to BMS · blank = disabled
  description      : abap.char(80);

}
