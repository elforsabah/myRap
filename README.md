extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend  behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
action ( precheck, features : global ) adjusttime parameter ZAE_D_adjusttime_AB result [0..*] $self;
action ( precheck, features : global ) terminateservice parameter ZAE_D_TERMINATE_SERVICE result [0..*] $self;

function GetDefaultsForTerminateservice
    result [1] ZAE_D_TERMINATE_SERVICE;

 determination Setfachbereich on modify { create; update; }


 side effects {

 action adjusttime affects $self, entity _ExtCustom;
 action assignworkarea affects $self;
 action terminateservice affects $self;

 }


}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
    zz_vehicleinfo = zz_vehicleinfo;
    zz_discrepancy = zz_discrepancy;
    zz_reactiontime =  zz_reactiontime;
    zz_order_date = zz_order_date;
    zz_timeadjustment = zz_timeadjustment;
 }

field ( readonly : update ) zz_tech_fachbe;
field ( readonly : update ) zz_vehicleinfo;
field ( readonly : update ) zz_discrepancy;
field ( readonly : update ) zz_reactiontime;
field ( readonly : update ) zz_order_date;
field ( readonly : update ) zz_timeadjustment;
}

extension for projection implementation in class zbp_e_bp_c_pdmnlservicewr_ext unique;

extend behavior for Service
{
use action assignworkarea;
use action adjusttime;
use action terminateservice;
use function GetDefaultsForTerminateservice;

 // Now the compiler will accept this because we are in the UI layer!
  side effects {
    action adjusttime affects entity _TourForRefresh;
    action terminateservice affects $self;
  }
}
