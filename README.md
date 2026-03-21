@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{
  @EndUserText.label: 'Grund'
  @ObjectModel.text.element: ['GrundText']
  @UI.textArrangement: #TEXT_ONLY
  @Consumption.valueHelpDefinition: [
    {
      entity          : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Conftype' },
      additionalBinding: [{ localElement: 'GrundText', element: 'Vtext', usage: #RESULT }], 
      useForValidation: true
    }
  ]
  @Consumption.defaultValue: 'PLSTORNO'   /* <-- 1. Prefills the dropdown automatically */
  @ObjectModel.mandatory: true            /* <-- 2. Keeps it editable so Fiori actually applies the default! */
  definiertegrund: abap.char( 8 );

  @UI.hidden: true
  GrundText : abap.char( 255 );

  @EndUserText.label: 'Grund für Storno (Bemerkung)'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
  stornogrund : abap.string( 0 ); 
}
