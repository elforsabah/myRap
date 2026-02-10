@Metadata.layer: #PARTNER
annotate entity /PLCE/C_PDMNLTourWR with
{
  // ... Existing actions ...

  @UI.hidden: true
  TourUUID;

  // --- Requirement 3: Remaining Duration UI ---
  @EndUserText.label: 'Remaining Duration'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]
  @UI.fieldGroup: [{ position: 101, qualifier: 'DefaultInformation' }]
  zz_remaining_duration;

  // --- Requirement 2: Capacity New with Progress Bar ---
  @EndUserText.label: 'Capacity Used' 
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Capacity used', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Capacity used', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint: {
    visualization: #PROGRESS,
    targetValue: 100, // Fixed target as requested
    criticality: 'TourCapacityColorValue' // Reuse existing criticality logic
  }
  @UI.fieldGroup: [{ position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;
}
