managed implementation in class zbp_i_wr_weighingsession unique;
strict;

with draft;

define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table zwr_weighsession
lock master
total etag Grossweight
authorization master ( instance )
etag master Grossweight
{
create; 
update; 
delete;

draft action Edit; 
draft action Activate; 
draft action Discard;

action NextStep result [1] $self; // server validates & increments Step 
action Submit result [1] $self; // final checks; printing trigger optional

validation validateStep1 on save { field Vbeln, Sessionid; } // Identification 
validation validateStep2 on save { field LoadType; } // Load type 
determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math
  
}
