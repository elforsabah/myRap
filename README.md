managed implementation in class zbp_i_wr_weighbridge unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHBRIDGE  alias WeighingSession
persistent table ZWR_WEIGHSESSI_D
draft table ZWR_WEIGHSESSI_D
lock master
total etag SalesDocument
authorization master ( instance )
etag master SalesDocument

{
//  Keys must be readonly in strict draft
field (readonly) SalesDocument; // <-- use your exact CDS element names
create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIGHINGSESSION result [1] $self;
action setloadType parameter ZAE_WR_WEIGHINGSESSION  result [1] $self;

//validation validateStep1 on save { field SalesDocument ;} // Identification
//validation validateStep2 on save { field Loadtype; } // Load type
//validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH
//
//determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math


}
