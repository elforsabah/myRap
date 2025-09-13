@Metadata.layer: #CUSTOMER
@UI: {
lineItem: [{ criticality: 'point_of_origin' }]
}
annotate entity /PLCE/C_PDMNLServiceWR with
{
@UI.hidden: true
@UI.criticalityRepresentation: #WITHOUT_ICON
@EndUserText.label: 'Point of Origin'
point_of_origin;
@UI.lineItem: [ { position: 70, importance: #HIGH },
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
}
