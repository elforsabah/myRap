<img width="460" height="404" alt="image" src="https://github.com/user-attachments/assets/c0c0042b-e115-45f9-9387-a27d09577f2d" />


@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{
  @EndUserText.label: 'Grund: STORNO DURCH DISPO'
  @ObjectModel.text.element: ['GrundText']
  @UI.textArrangement: #TEXT_ONLY
  @Consumption.valueHelpDefinition: [
    {
      entity          : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Conftype' },
      additionalBinding: [{ localElement: 'GrundText', element: 'Bemerkung', usage: #RESULT }], 
      useForValidation: true
    }
  ]
//  @ObjectModel.readOnly: true
  definiertegrund: abap.char( 20 );
//
  @UI.hidden: true
  GrundText : abap.char( 255 );

  @EndUserText.label: 'Bemerkung'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
  stornogrund : abap.string( 0 ); 
}
