why can't I extend the view and add new fields, 

why is it possible to add only associations ? 

@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'PD Service Extension Custom'
@AbapCatalog.extensibility: { extensible: true } 
@ObjectModel.usageType:{
  serviceQuality: #A,
  sizeCategory: #L,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/R_PDServiceExtCustom
  as select from /plce/tpdsrvcst
  association to parent /PLCE/R_PDService as _Service on $projection.ServiceUUID = _Service.ServiceUUID
{
  key service_uuid as ServiceUUID,
      _Service
}
****
