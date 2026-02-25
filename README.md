behavior for /PLCE/C_PDMNLServiceWR alias Service
{
  // ... your other definitions ...

  // Tell the UI that running this action affects the parent Tour
  side effects {
    action adjusttime affects entity _Tour; // Replace _Tour with your actual association name to the Tour entity
  }
}
