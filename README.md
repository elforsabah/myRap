<img width="1548" height="606" alt="image" src="https://github.com/user-attachments/assets/7227047e-f5e3-4b1a-af56-3cd038a665d0" />

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
   
   
   /PLCE/R_PDService._ExtCustom.zz_main_position, // Hauptposition
   
   
   _TourForRefresh 
}



<img width="302" height="61" alt="image" src="https://github.com/user-attachments/assets/dd4660bb-d8e9-48d5-ad77-d8c7e3914d64" />
<img width="607" height="265" alt="image" src="https://github.com/user-attachments/assets/fef48f5b-115d-43fa-ae5c-dbdf0f960743" />
<img width="460" height="457" alt="image" src="https://github.com/user-attachments/assets/6db42fd2-fbad-48ea-a4a8-67f2581eae79" />
<img width="548" height="318" alt="image" src="https://github.com/user-attachments/assets/d2a22521-4a90-4b41-8172-3ed74b2445b4" />
<img width="476" height="225" alt="image" src="https://github.com/user-attachments/assets/cd82632a-6756-44b9-83b0-a6aabf90ffba" />
<img width="390" height="196" alt="image" src="https://github.com/user-attachments/assets/3d799f92-b402-40f4-958f-af661140e24d" />





