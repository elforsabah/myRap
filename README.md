extend view entity /PLCE/R_PDServiceExtCustom with
{
  /plce/tpdsrvcst.zz_tech_fachbe,  //Fachbereich
  /plce/tpdsrvcst.zz_discrepancy,  // Termin-Diskrepanz
  /plce/tpdsrvcst.zz_reactiontime, // “Reaktionszeit
  /plce/tpdsrvcst.zz_vehicleinfo,  // Fahrzeuginfo
  /plce/tpdsrvcst.zz_order_date,   // Auftragsdatum
  /plce/tpdsrvcst.zz_timeadjustment, // Zeitanpassung
  /plce/tpdsrvcst.zz_pobjnr_main, // Hauptposition
  /plce/tpdsrvcst.wdplantnr //Entsorgungsanlage 
}


extend view entity /PLCE/C_PDMNLServiceWR with

 // 1. DEFINE the Association (The Bridge)
   association [0..1] to /PLCE/C_PDMNLTourWR as _TourForRefresh 
      on $projection.TourId = _TourForRefresh.TourId
 {

   /PLCE/R_PDService._ExtCustom.zz_tech_fachbe, //as Fachberei
   /PLCE/R_PDService._ExtCustom.zz_discrepancy,  // Termin-Diskrepanz
   
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'  // Reference your new class
   virtual service_criticality : abap.int1,
   /PLCE/R_PDService._ExtCustom.zz_reactiontime, // “Reaktionszeit
   /PLCE/R_PDService._ExtCustom.zz_vehicleinfo,  // Fahrzeuginfo
   @UI.hidden: true
   /PLCE/R_PDService._ExtCustom.zz_timeadjustment, // Zeitanpassung
   
   // 2. The NEW VIRTUAL field (Formatted Text). 
   // This will hold the "+37 MIN" string for the UI.
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
   @EndUserText.label: 'Zeitanpassung in Min'
   virtual zz_timeadjust_display : abap.char( 35 ),
   
  @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition Kritikalität'
  virtual zz_main_pos_criticality : abap.int1,
  
//   @ObjectModel.virtualElement: true
//   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
//   @EndUserText.label: 'Hauptposition anzeigen'
//  virtual zz_main_position : zwr_d_pobjnr_main,
  
  @EndUserText.label: 'Hauptposition '
  @UI.selectionField: [{ position: 1000 }]
//  @UI.lineItem: [{ position: 10000]
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  @EndUserText.label: 'Entsorgungsanlage'
   /PLCE/R_PDService._ExtCustom.wdplantnr,
   _TourForRefresh 
}




extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend behavior for Service
{

  action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
  action ( precheck, features : global ) adjusttime parameter ZAE_D_adjusttime_AB result [0..*] $self;

  action ( precheck, features : global ) anlageaendern parameter ZAE_D_ANLAGE_AENDERN_AB result [0..*] $self;



  action ( precheck, features : global ) terminateservice
    parameter ZAE_D_TERMINATE_SERVICE
    result [0..*] $self;


  //determination determination PopulateExtCustomFromOrder on modify { create; update; }
determination PopulateExtCustomFromOrder on modify
{
  create;
  update;

}



  side effects
  {

    action adjusttime affects $self, entity _ExtCustom;
    action assignworkarea affects $self;
    action terminateservice affects $self;
    action anlageaendern affects $self , entity _ExtCustom;
  }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
  mapping for /plce/tpdsrvcst
    {
      zz_tech_fachbe    = zz_tech_fachbe;
      zz_vehicleinfo    = zz_vehicleinfo;
      zz_discrepancy    = zz_discrepancy;
      zz_reactiontime   = zz_reactiontime;
      zz_order_date     = zz_order_date;
      zz_timeadjustment = zz_timeadjustment;
      zz_pobjnr_main    = zz_pobjnr_main;
      WDPLANTNR         = WDPLANTNR;
    }

  field ( readonly : update ) zz_tech_fachbe;
  field ( readonly : update ) zz_vehicleinfo;
  field ( readonly : update ) zz_discrepancy;
  field ( readonly : update ) zz_reactiontime;
  field ( readonly : update ) zz_order_date;
  field ( readonly : update ) zz_timeadjustment;
  field ( readonly : update ) zz_pobjnr_main;
  field ( readonly : update ) WDPLANTNR;
}



extension for projection implementation in class zbp_e_bp_c_pdmnlservicewr_ext unique;

extend behavior for Service
{
use action assignworkarea;
use action adjusttime;
use action terminateservice ;
use action anlageaendern;


 // Now the compiler will accept this because we are in the UI layer!
  side effects {
    action adjusttime affects entity _TourForRefresh;
    action terminateservice affects $self;
   action anlageaendern affects $self;
  }
}



@Metadata.layer: #CUSTOMER
@UI: {
lineItem: [{ criticality: 'service_criticality' }]
}
annotate entity /PLCE/C_PDMNLServiceWR with

{
  @UI.lineItem: [
     { position: 190, importance: #HIGH },
     {
       type                : #FOR_ACTION,
       dataAction          : 'assignworkarea',   // bound action name
       label               : 'Arbeitsbereich zuweisen',
       iconUrl: 'sap-icon://wrench',
  //      requiresSelection   : false,              // => global action
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH

     },
     
     { 
       type                : #FOR_ACTION,
       dataAction          : 'adjusttime',   // bound action name
       label               : 'Zeitanpassung ändern',
       iconUrl: 'sap-icon://wrench',
//        requiresSelection   : false,              // => global action
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH
       
     }, 
     
     {
       type                : #FOR_ACTION,
       dataAction          : 'terminateservice',   // bound action name
       label               : 'Service stornieren',
       iconUrl: 'sap-icon://wrench',
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH

     },
     
          {
       type                : #FOR_ACTION,
       dataAction          : 'anlageaendern',   // bound action name
       label               : 'Anlage ändern',
       iconUrl: 'sap-icon://wrench',
       invocationGrouping  : #CHANGE_SET,        // optional
       emphasized          : true,               // optional
       position            : 100,
       importance          : #HIGH

     }
     
   ]
   
   
  
  @EndUserText.label: 'Fachbereich'
  @UI.selectionField: [{position: 20}]
  @Consumption.valueHelpDefinition: [
    {
      entity         : { name: 'ZZPD_WORKREA_VH', element: 'WorkArea' },
      label          : 'Workarea',
      useForValidation: true
    }
  ]
  zz_tech_fachbe;
  @EndUserText.label: 'Termin-Diskrepanz'
  zz_discrepancy; // Termin-Diskrepanz
  @EndUserText.label: 'Reaktionszeit'
  zz_reactiontime; // “Reaktionszeit
  @EndUserText.label: 'Fahrzeuginfo'
  zz_vehicleinfo;  // Fahrzeuginfo
  @EndUserText.label: 'Zeitanpassung in Min'
  @UI.lineItem: [{ position: 131, importance: #HIGH } ]
  zz_timeadjust_display; // Zeitanpassung
  @UI.hidden: true
  service_criticality;
//  @UI.hidden: true
  @EndUserText.label: 'main Criticallity'
  @UI.lineItem: [{ position: 135, importance: #HIGH , hidden: true } ]
  zz_main_pos_criticality;
  @EndUserText.label: 'Entsorgungsanlage'
  @UI.lineItem: [{ position: 132, importance: #HIGH } ]
  wdplantnr;
  @UI.lineItem: [{ position: 137, importance: #HIGH , criticality: 'zz_main_pos_criticality'} ]
  zz_pobjnr_main;
   
}




<img width="1125" height="905" alt="image" src="https://github.com/user-attachments/assets/fc7c622c-a931-417b-a886-7c9e8dbf3c68" />
