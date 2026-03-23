@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{
//  @EndUserText.label: 'Grund: STORNO DURCH DISPO'
  @ObjectModel.text.element: ['GrundText']
  @UI.textArrangement: #TEXT_ONLY
  @Consumption.valueHelpDefinition: [
    {
      entity          : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Conftype' },
      additionalBinding: [{ localElement: 'GrundText', element: 'Bemerkung', usage: #RESULT }], 
      useForValidation: true,
      
      label: 'Grund: STORNO DURCH DISPO'
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

<img width="522" height="413" alt="image" src="https://github.com/user-attachments/assets/cd357794-8763-4328-b395-f2f3c5b9b1cf" />
