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
