@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Result Work Area - Service'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/P_PDWorkAreaService
  as select from /PLCE/P_PDWorkAreaServiceFunc( p_client: $session.client )
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
  key service_uuid as ServiceUUID

}
