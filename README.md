@Metadata.layer: #PARTNER
annotate entity /PLCE/C_PDMNLTourWR with
{
  // ... (Previous annotations) ...

  @EndUserText.label: 'Restdauer'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]  
  @UI.fieldGroup: [{ position: 101, qualifier: 'DefaultInformation' }]
  zz_remaining_duration;
 
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Kapazitätsauslastung', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Kapazitätsauslastung', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint:{
    visualization : #PROGRESS,
    // CRITICAL CHANGE: Use targetValueElement instead of fixed targetValue
    // This compares tour_capacity_new (230) against TourDuration (510)
    targetValueElement: 'TourDuration', 
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;
}

