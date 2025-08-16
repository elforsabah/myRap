define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table ZWR_WEIGHSESSION
lock master
{
  " Keys must be readonly in strict draft
  field (readonly) Vbeln, Sessionid;   " <-- use your exact CDS element names

  create;
  update;
  delete;

  " Mandatory draft actions in strict mode
  draft action Edit;
  draft action Activate;
  draft action Discard;
  draft action Resume;                 " <-- required
  draft determine action Prepare;      " <-- required

  " Your custom actions / logic
  action NextStep  result [1] $self;
  action Submit    result [1] $self;

  validation validateStep1 on save { field Vbeln, Sessionid; }
  validation validateStep2 on save { field LoadType; }
  determination calcNet on modify { field Grossweight, Tareweight; }
}
