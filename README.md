create; update; delete;

  draft action Edit;
  draft action Activate;
  draft action Discard;

  action NextStep  result [1] $self;   // server validates & increments Step
  action Submit    result [1] $self;   // final checks; printing trigger optional

  validation validateStep1 on save { field Vbeln, Sessionid; }    // Identification
  validation validateStep2 on save { field LoadType; }            // Load type
  determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math
