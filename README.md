extend view entity /PLCE/C_PDMNLServiceWR with {
   // ... (Your other fields like service_criticality) ...

   // 1. The REAL field (Pure Number). Hide it from the UI.
   @UI.hidden: true
   /PLCE/R_PDService._ExtCustom.zz_timeadjustment, 

   // 2. The NEW VIRTUAL field (Formatted Text). 
   // This will hold the "+37 MIN" string for the UI.
   @ObjectModel.virtualElement: true
   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
   @EndUserText.label: 'Zeitanpassung in Min'
   virtual zz_timeadjust_display : abap.char( 15 )
}
