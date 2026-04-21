@EndUserText.label: 'Abstract View für Arbeitbereich'

define abstract entity ZAE_D_ANLAGE_AENDERN_AB
 
{
  @EndUserText.label: 'Zeitanpassung'
   ZeitInMin : abap.char( 35 ); 
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
