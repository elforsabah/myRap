@Metadata.layer: #CUSTOMER

annotate entity /PLCE/C_PDMNLServiceWR with
 

   { @UI.lineItem: [ { position: 70, importance: #HIGH },
                        { 
                        type:#FOR_ACTION,
                        dataAction:'changeServiceType',
                        label: 'Change ServiceType',
                        iconUrl: 'sap-icon://wrench',
                        inline: true,
                        emphasized: true,
                        importance: #HIGH,
                        position: 100,
                        invocationGrouping: #CHANGE_SET}] 
  @UI.textArrangement: #TEXT_ONLY
  @UI.fieldGroup: [{position: 50, qualifier: 'DefaultInformation' }]
  @EndUserText.label: 'Service Type'
  ServiceType;
 
  @UI.lineItem: [{ criticality: 'point_of_origin' }]
  @EndUserText.label: 'Point of Origin'
  point_of_origin;
}
