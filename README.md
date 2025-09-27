@EndUserText.label: 'Abstract View für Arbeitsbereich'
define abstract entity ZAE_D_WORKAREA_AB {
  @EndUserText.label: 'Neue Arbeitsbereich'

  @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'Z_I_WorkareaVH', element: 'WorkArea' },
      label          : 'Workarea',
      valueListType  : #FIXED_VALUES,         // <-- tells FE to use a ComboBox
      display        : #VALUE_DESCRIPTION     // value + text in the drop-down
    }
  ]
  WorkArea : /plce/pdwork_area;
}
