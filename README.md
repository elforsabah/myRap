determination PopulateExtCustomFromOrder on modify 
{ 
  create;
  field ServiceStatus;          // ✅ BEST - changes frequently
  field PlanningStatus;         // ✅ GOOD - changes during planning
  field ServicePriority;        // ✅ OK - might change
  field RequestedDate;          // ✅ OK - changes when rescheduling
  field EarliestDate;           // ✅ OK - changes when rescheduling
  field LatestDate;             // ✅ OK - changes when rescheduling
}
