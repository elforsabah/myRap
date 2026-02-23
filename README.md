define view entity ZI_TOUR_SVC_SUM_WR
  as select from /PLCE/C_PDMNLTourServiceAsgWR as asg
    inner join   /PLCE/C_PDMNLServiceWR        as svc on asg.ServiceUUID = svc.ServiceUUID
{
  key asg.TourUUID,
      
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      sum(svc.TotalDuration) as TotalServiceDuration,
      
      // 1. Remove ' MIN'
      // 2. Remove '+' (Keep '-' because CAST handles negative numbers)
      // 3. Cast the clean string to an Integer
      // 4. Sum the result
      sum( cast( 
             replace( 
               replace( svc.zz_timeadjustment, ' MIN', '' ), 
             '+', '' ) 
           as abap.int4 ) 
         ) as TotalTimeAdjustment,
      
      svc.TotalDurationUnit
}
group by
  asg.TourUUID,
  svc.TotalDurationUnit
