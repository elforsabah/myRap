define view entity ZI_TOUR_SVC_SUM_WR
  as select from /PLCE/R_PDTourServiceAsg as asg
    inner join   /PLCE/R_PDService        as svc on asg.ServiceUUID = svc.ServiceUUID
{
  key asg.TourUUID,
      
      @Semantics.quantity.unitOfMeasure: 'TotalDurationUnit'
      sum(svc.TotalDuration) as TotalServiceDuration,
      
      // Look how clean this is now! Natively sums the pure numbers.
      sum(svc._ExtCustom.zz_timeadjustment) as TotalTimeAdjustment,
      
      svc.TotalDurationUnit
}
group by
  asg.TourUUID,
  svc.TotalDurationUnit
