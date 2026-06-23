@Metadata.layer: #CUSTOMER
annotate view /PLCE/C_PDMNLTour with
{
  @UI.lineItem: [{
    position: 995,
    label: 'BMS-Status',
    criticality: 'ZzBmsCriticality',
    criticalityRepresentation: #WITH_ICON
  }]
  @EndUserText.label: 'BMS-Status'
  ZzBmsStatus;

  @UI.hidden: true
  ZzBmsCriticality;
}
