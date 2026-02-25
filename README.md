extension;

// Note: Replace 'ServiceWR' with whatever Alias the standard Projection BDEF uses
extend behavior for ServiceWR 
{
  // Now the compiler will accept this because we are in the UI layer!
  side effects {
    action adjusttime affects entity _TourForRefresh;
  }
}
