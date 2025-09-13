@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'EWA_ORDER_OBJECT'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_WR_EWA_ORDER_OBJECT as select distinct from ewa_order_object as _EWA_ORDER_OBJECT
{
    key ordernr as Ordernr,
    key order_laufnr as OrderLaufnr,
    key /plcp/pd_service_uuid as PdServiceUuid,
    pobjnr as EWAWasteDsplOrdItmObjectNumber, 
    container as Container,
    /plcp/tidnr_rm as /plcp/tidnrRm,
    /plcp/tidnr_new_rm as /plcp/tidnrNewRm,
    /plcp/blocking_conf_note as /plcp/blockingConfNote,
     zzpoint_origin_wdoi as point_of_origin
} 
