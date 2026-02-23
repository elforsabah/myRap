@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Aggregated Service Durations per Tour'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #X,
  sizeCategory: #S,
  dataClass: #MIXED
}
// 1. Remove "root" - this is just a helper view
define view entity ZI_TOUR_SVC_SUM_WR
  // 2. Use the underlying R_ (or I_) views instead of C_ views
  as select from /PLCE/R_PDTourServiceAsg as asg 
    inner join   /PLCE/R_PDService        as svc on asg.ServiceUUID = svc.ServiceUUID
{
  key asg.TourUUID,
      
      // Summing up the fields directly in the database
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      sum(svc.TotalDuration)     as TotalServiceDuration,
      
      // 1. Remove ' MIN'
      // 2. Remove '+' (Keep '-' because CAST handles negative numbers)
      // 3. Cast the clean string to an Integer
      // 4. Sum the result
      // NOTE: If zz_timeadjustment is in the extension association at this layer, 
      // you might need to use svc._ExtCustom.zz_timeadjustment instead.
      sum( cast( 
             replace( 
               replace( svc.zz_timeadjustment, ' MIN', '' ), 
             '+', '' ) 
           as abap.int4 ) 
         ) as TotalTimeAdjustment,
      
      // Required for quantity fields in aggregated views
      svc.TotalDurationUnit
}
group by
  asg.TourUUID,
  svc.TotalDurationUnit
