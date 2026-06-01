@EndUserText.label: 'Abstract View für Entsorgungsanlage'

define abstract entity ZAE_D_ANLAGE_AENDERN_AB
 
{
  @EndUserText.label: 'Entsorgungsanlage'
 @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZAE_D_ENTSORGUNG_ANLAGE_VH', element: 'Entsorgungsanlage' },
      label          : 'Entsorgungsanlage'
    }
  ]
   anlage : ewael_wdplantnr; 
}
