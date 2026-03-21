extend behavior for Service
{
  action ( precheck, features : global ) assignworkarea
    parameter ZAE_D_WORKAREA_AB result [0..*] $self;

  action ( precheck, features : global ) adjusttime
    parameter ZAE_D_adjusttime_AB result [0..*] $self;

  action ( precheck, features : global ) terminateservice
    parameter ZAE_D_TERMINATE_SERVICE result [0..*] $self;

  function GetDefaultsForTerminateservice
    result [1] ZAE_D_TERMINATE_SERVICE;

  determination Setfachbereich on modify { create; update; }

  side effects {
    action adjusttime affects $self, entity _ExtCustom;
    action assignworkarea affects $self;
    action terminateservice affects $self;
  }
}
