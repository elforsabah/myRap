extend view entity /PLCE/R_PDServiceExtCustom with
association [0..1] to ZI_WR_EWA_ORDER_OBJECT as _ORDER_OBJECT on _ORDER_OBJECT.PdServiceUuid = /plce/tpdsrvcst.service_uuid
{
  /plce/tpdsrvcst.zz_tech_fachbe,  //Fachbereich
  /plce/tpdsrvcst.zz_discrepancy,  // Termin-Diskrepanz
  /plce/tpdsrvcst.zz_reactiontime, // “Reaktionszeit
  /plce/tpdsrvcst.zz_vehicleinfo,  // Fahrzeuginfo
  /plce/tpdsrvcst.zz_order_date,   // Auftragsdatum
  /plce/tpdsrvcst.zz_timeadjustment, // Zeitanpassung
  /plce/tpdsrvcst.zz_pobjnr_main, // Hauptposition
  /plce/tpdsrvcst.wdplantnr, //Entsorgungsanlage -> To be updated 
  _ORDER_OBJECT.Entsorgunganlage //Entsorgungsanlage -> To be updated from Ewav_object table
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Helper CDS for EWA-ORDER_ObJECT'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_WR_EWA_ORDER_OBJECT as select distinct from ewa_order_object as _EWA_ORDER_OBJECT
association [0..1] to /PLCE/R_PDService  as _ServiceCore on _ServiceCore.ReferenceInternalId = $projection.EWAWasteDsplOrdItmObjectNumber

{
    key ordernr as Ordernr,
    key order_laufnr as OrderLaufnr,
    key /plcp/pd_service_uuid as PdServiceUuid,
    pobjnr as EWAWasteDsplOrdItmObjectNumber,
    wdplantnr as Entsorgunganlage,
    zz_pobjnr_main as Main_position,
    zz_order_date  as late_date,
    _ServiceCore.ReferenceId as Referenceid,
     zz_reactiontime as Reaction_Time
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
  
   @ObjectModel.virtualElement: true
  @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
  @EndUserText.label: 'Hauptposition'
  virtual zz_main_position : zwr_d_pobjnr_main,
  
  @EndUserText.label: 'Hauptposition '
//  @UI.selectionField: [{ position: 1000 }]
   @UI.lineItem: [{ hidden: true }]
  /PLCE/R_PDService._ExtCustom.zz_pobjnr_main,
  
  
//   @ObjectModel.virtualElement: true
//   @ObjectModel.virtualElementCalculatedBy: 'ABAP:ZCL_WR_SERVICE_EXTEND_CALC'
//   virtual zz_entsorgunganlage :  ewael_wdplantnr, // Entsorgungsanlage
//  
coalesce( 
  /PLCE/R_PDService._ExtCustom.wdplantnr,  // ← First choice
  /PLCE/R_PDService._ExtCustom._ORDER_OBJECT.Entsorgunganlage            // ← Fallback
) as zz_entsorgunganlage,
   
  
  
   /PLCE/R_PDService._ExtCustom.wdplantnr,
   _TourForRefresh 
}
