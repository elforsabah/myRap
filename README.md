@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Extended Result Work Area - Service'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
serviceQuality: #C,
sizeCategory: #XL,
dataClass: #TRANSACTIONAL
}
define view entity ZE_P_PDWorkAreaServiceExt
as select from ZE_P_PDWorkAreaServiceFuncExt( p_client: $session.client )
association to parent /PLCE/R_PDServiceExtCustom as _ExtCustom
on $projection.ServiceUUID = _ExtCustom.ServiceUUID
{
@Consumption.valueHelpDefinition: [{
entity: {
name    : '/PLCE/C_PDWorkAreaUser_VH',
element : 'WorkArea'
},
label     : 'Work Area',
useForValidation: true
}]
key work_area    as WorkArea,
key service_uuid as ServiceUUID,
_ExtCustom  // To-parent association
}
