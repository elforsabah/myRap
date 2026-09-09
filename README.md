@Metadata.layer: #PARTNER

annotate entity /PLCE/C_PDMNLTourWR
    with 
{
    @UI.lineItem: [{ position: 1 },
                 { hidden: true },
                { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Spur ermitteln', invocationGrouping: #CHANGE_SET, position: 40  },
                 { type:#FOR_ACTION, dataAction:'releaseTour', label:'Tour Freigeben', invocationGrouping: #CHANGE_SET, position: 30  },
                 { type:#FOR_ACTION, dataAction:'recallTour', label:'Tour zurück rufen', invocationGrouping: #CHANGE_SET, position: 25 },
                 { type:#FOR_ACTION, dataAction:'createtour', label:'Tour anlegen', invocationGrouping: #CHANGE_SET, position: 10 },
                 { type:#FOR_ACTION, dataAction:'tourgenerateDocument', label:'Ducument drucken', invocationGrouping: #CHANGE_SET, position: 12 },
                 { type:#FOR_ACTION, dataAction:'touranBMSfreigeben', label:'Tour an BMS freigeben', invocationGrouping: #CHANGE_SET, position: 11 },
                 { type:#FOR_ACTION, dataAction:'stornoBMSService', label:'BMS storno', invocationGrouping: #CHANGE_SET, position: 11 },
                 { type:#FOR_ACTION, dataAction:'deleteTour', label:'Tour Löschen', invocationGrouping: #CHANGE_SET, position: 20 },
                 { type:#FOR_ACTION, dataAction:'SequenceAutomated', label: 'Tour automatisch ordnen', position: 50 },
                 { type:#FOR_ACTION, dataAction:'TransferSequence', label:'Servicereihfolge übertragen', invocationGrouping: #CHANGE_SET, position: 70 },
                 { type:#FOR_ACTION, dataAction:'GenerateGeoRoute', label:'Spur ermitteln', invocationGrouping: #CHANGE_SET, qualifier: 'DefaultMap' }
                        
               ]                             
//   
   @UI.hidden: true
    TourUUID; 
 
    // Remaining Duration (e.g. 280)
  @EndUserText.label: 'Restdauer(freie Kapazität)'
  @UI.lineItem: [{ position: 101, importance: #HIGH }]  
  zz_remaining_duration;
 
  // Capacity Used (e.g. 230)
  @EndUserText.label: 'Kapazitätsauslastung(in Min)'
  @UI.lineItem: [
    { position: 102, type: #AS_DATAPOINT, label: 'Kapazitätsauslastung(in Min)', importance: #HIGH },
    { position: 50,  type: #AS_DATAPOINT, label: 'Kapazitätsauslastung(in Min)', importance: #HIGH, qualifier: 'DefaultMap' }
  ]
  @UI.dataPoint: {
    visualization: #PROGRESS,
    targetValueElement: 'MaximumTourDuration_Converted', 
//    criticality: 'TourCapacityColorValue'
    criticality: 'zz_capacity_criticality'
  }
  @UI.fieldGroup: [{position: 110, qualifier: 'DefaultInformation', type: #AS_DATAPOINT }]
  tour_capacity_new;
  
  @UI.hidden: true
  tour_duration_min;
  
  @UI.hidden: true
  @UI.lineItem: [{ position: 450, importance: #HIGH }] 
  TourDuration;

  @UI.hidden: true
  MaximumTourDuration_Converted;
  
  // 1. Hide the standard SAP field (Hours)
//  @UI.hidden: true
  @UI.lineItem: [{ position: 121, importance: #HIGH }]
  DrivingTime;

  // 2. Show your new custom field (Minutes)
  @UI.lineItem: [{ position: 122, importance: #HIGH }]
  @EndUserText.label: 'Fahrzeit'  // Driving Time
  zz_driving_time_min;
  
  
  @UI.lineItem: [{
    position: 995,
    label: 'BMS-Status',
    criticality: 'ZzBmsCriticality',
    criticalityRepresentation: #WITH_ICON
  }]
  @EndUserText.label: 'BMS-Status'
  zz_bms_status;

@UI.hidden: true
  ZzBmsCriticality;
}
