extend behavior for Service
{
  association _WorkAreaServicesExt { with draft; }  // Expose the new association
  action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
}
