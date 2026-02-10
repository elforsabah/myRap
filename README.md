@Metadata.layer: #PARTNER
annotate entity /PLCE/C_PDMNLTourWR with
{
  // ... (Other fields) ...

  // Remaining Duration (e.g. 280)
  @EndUserText.label: 'Restdauer'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]  
  zz_remaining_duration;
 
  // Capacity Used (e.g. 230)
  @EndUserText.label: 'Kapazitätsauslastung'
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Kapazitätsauslastung', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Kapazitätsauslastung', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint: {
    visualization: #PROGRESS,
    // Compares 230 (Used Min) vs 510 (Total Min)
    targetValueElement: 'TourDuration', 
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;
}
