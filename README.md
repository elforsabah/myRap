@Metadata.layer: #CUSTOMER
annotate view /PLCE/C_PDTour with
{
  @UI.lineItem: [
    {
      type: #FOR_ACTION,
      dataAction: 'stornoBMSService',
      label: 'BMS Storno',
      position: 50
    }
  ]
  TourUUID;
}
