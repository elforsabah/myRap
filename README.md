action ( precheck, features : global ) terminateservice
  parameter ZAE_D_TERMINATE_SERVICE
  result [0..*] $self
  { default function GetDefaultsForTerminateService; }
