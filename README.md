@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Value Help für Storno'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@ObjectModel.resultSet.sizeCategory: #XS
define view entity ZAE_D_TERMINATE_SERVICE_VH as select from tewaconftypet
{
  @ObjectModel.text.element: [ 'Vtext' ]
  key conftype as Conftype,
//  @UI.hidden: true
  key spras as Spras,
  @Semantics.text: true
  @UI.textArrangement: #TEXT_ONLY
  vtext as Vtext
}where spras = $session.system_language



@EndUserText.label: 'Abstract View für Arbeitbereich'

define abstract entity ZAE_D_TERMINATE_SERVICE
{

 @EndUserText.label: 'Grund'
   @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Vtext' },
      label          : 'Storno',
      useForValidation: true
    }
  ]
  definiertegrund: abap.char( 255 );
  @EndUserText.label: 'Grund für Storno'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
   stornogrund : abap.string( 0 ); 
}
