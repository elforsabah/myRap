@Metadata.layer: #CUSTOMER
annotate view /PLCE/C_PDTour with
{
  @UI.lineItem: [{ position: 999, label: 'BMS-Status' }]
  @UI.fieldGroup: [{ qualifier: 'BMS', position: 10, label: 'BMS-Status' }]
  @EndUserText.label: 'BMS-Status'
  @UI.textArrangement: #TEXT_ONLY
  ZzBmsStatus;

  @UI.lineItem: [{
    position: 998,
    criticality: 'ZzBmsCriticality',
    criticalityRepresentation: #WITH_ICON,
    label: ' '
  }]
  ZzBmsCriticality;
}
