@EndUserText.label: 'Abstract View für Arbeitbereich'
define abstract entity ZAE_D_TERMINATE_SERVICE
{
  @EndUserText.label: 'Grund: STORNO DURCH DISPO'
  @Consumption.valueHelpDefinition: [
    {
      entity            : { name: 'ZAE_D_TERMINATE_SERVICE_VH', element: 'Conftype' },
      additionalBinding : [
        { localElement: 'stornogrund', element: 'Bemerkung', usage: #RESULT }
      ],
      useForValidation  : true
    }
  ]
  definiertegrund : abap.char(20);

  @EndUserText.label: 'Bemerkung'
  @UI.multiLineText: true
  @ObjectModel.mandatory: true
  stornogrund : abap.string(0);
}

