@EndUserText.label: 'Tour Template → BMS Kolonne Mapping'
@AbapCatalog.enhancement.category: #NOT_EXTENSIBLE
@AbapCatalog.tableCategory: #TRANSPARENT
@AbapCatalog.deliveryClass: #C
@AbapCatalog.dataMaintenance: #ALLOWED
define table ztour_bms_kolonne {

  key mandt         : mandt not null;
  key tour_template : /plce/pdtour_template not null;
  bms_team          : text40;

}
