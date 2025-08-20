@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Blocked Vehicles Interface View'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true

define root view entity ZI_PDMA_BLOCKEDVEHICLE as select from ztpdmavehicleres

{

    key vehicleid             as VehicleId,
      driverresourceid      as DriverResourceId,
      //timestamp             as Timestamp,
      @Semantics.user.createdBy: true     
      created_by            as CreatedBy, 
      @Semantics.systemDateTime.createdAt: true     
      created_at            as CreatedAt, 
      @Semantics.user.lastChangedBy: true  
      last_changed_by       as LastChangedBy,
      @Semantics.systemDateTime.lastChangedAt: true
      last_changed_at       as LastChangedAtwhat    
}
