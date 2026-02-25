extension for projection implementation in class zbp_e_bp_c_pdmnlservicewr_ext unique;

extend behavior for Service
{
use action assignworkarea;
use action adjusttime;

 // Now the compiler will accept this because we are in the UI layer!
  side effects {
    action adjusttime affects entity _TourForRefresh;
  }
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
   
   _TourForRefresh 
}
