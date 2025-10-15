  @UI.lineItem: [{ position: 110, type : #AS_DATAPOINT, label: 'Capacity used (in %)', importance: #HIGH },
                 { position: 50, type : #AS_DATAPOINT, label: 'Capacity used (in %)', importance: #HIGH, qualifier: 'DefaultMap' }]
  @UI.dataPoint:{
    visualization : #PROGRESS,
    targetValue: 100,
    criticality: 'TourCapacityColorValue'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
