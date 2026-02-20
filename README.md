@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Planning Service with Waste Order Object'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity Z_PDService_EwaOrderObj
  as select from /PLCE/R_PDService as Service
  
  // Create the association to the transparent table using the requested keys
  association [0..1] to ewa_order_object as _WasteOrderObject 
    on Service.ReferenceInternalId = _WasteOrderObject.pobjnr

{
      // Key and Core Service Fields
  key Service.ServiceUUID,
      Service.ServiceId,
      Service.Profile,
      Service.Action,
      Service.ServiceType,
      Service.ServiceStatus,
      Service.PlanningStatus,
      
      // Link Fields
      Service.ReferenceId,
      Service.ReferenceInternalId,
      
      // Select fields from EWA_ORDER_OBJECT via the association
      _WasteOrderObject.ordernr        as WasteOrderNumber,
      _WasteOrderObject.container      as ContainerSerial,
      _WasteOrderObject.waste_type     as WasteType,
      _WasteOrderObject.route_sequence as RouteSequence,
      _WasteOrderObject.pobjnr         as WasteObjectNumber,

      // Expose the association itself for UI/OData consumption or extending
      _WasteOrderObject
}
