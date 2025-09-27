@Metadata.layer: #CUSTOMER
annotate entity /PLCE/C_PDMNLServiceWR
with @(
  UI.LineItem: [
    {
      type: #FOR_ACTION,
      dataAction: '<YourODataService>.EntityContainer/assignworkarea',
      label: 'Assign Work Area',
      iconUrl: 'sap-icon://wrench',
      emphasized: true,
      importance: #HIGH,
      position: 100,
      invocationGrouping: #CHANGE_SET
    }
  ]
)
{
  @EndUserText.label: 'Fachbereich'
  Fachbereich;
}
