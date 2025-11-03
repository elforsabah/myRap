managed implementation in class /plce/bp_r_pdservice unique;
strict ( 2 );
extensible
{
  with determinations on modify;
  with validations on save;
}
with draft;

define behavior for /PLCE/R_PDService alias Service
persistent table /plce/tpdsrv
draft table /plce/dpdsrv
lock master total etag LastChangedAt
authorization master ( global )
etag master LocalLastChangedAt
extensible
{
  create;
  update;
  delete ( features : instance );

  field ( readonly, numbering : managed ) ServiceUUID;

  association _ServiceTask { create ( features : instance ); with draft; }
  association _Attachments { create; with draft; }
  association _Notes { create; with draft; }
  association _StatusHistory { create; with draft; }

  association _ExtUniversal { create; with draft; }
  association _ExtWaste { create; with draft; }
  association _ExtCustom { create; with draft; }

  action ( features : instance, precheck ) assignTour parameter /PLCE/D_PDTourP result [0..*] $self;
  //action ( features : instance, precheck ) assignTour parameter /PLCE/D_PDTourP result [1] $self; //error in invoke action edit flow (accepts only 1 result)
  action ( features : instance, precheck ) createTour parameter /PLCE/D_PDTourCreate result [0..*] $self; ///PLCE/D_PDTourCreate ///PLCE/D_PDTourTemplateP
  //action ( features : instance ) planAutomatically parameter /PLCE/D_PDSPlanAutomaticallyP; ///PLCE/D_PDPlanAutomaticallyP;

  action ( features : instance ) completeService;
  action ( features : instance ) confirmService;
//  action ( features : instance ) TransferSequence;





extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend behavior for Service
{


action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
// Precheck validates input; action updates ExtCustom.zz_tech_fachbe and returns updated Service

 determination Setfachbereich on modify { create; update; }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
 }

field ( readonly : update ) zz_tech_fachbe;

}


extension for projection implementation in class zbp_e_bp_c_pdmnlservicewr_ext unique;

extend behavior for Service
{
use action assignworkarea;
use  delete;
//use association _WorkAreaServicesExt;
}

extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend behavior for Service
{

use delete;
action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
// Precheck validates input; action updates ExtCustom.zz_tech_fachbe and returns updated Service

 determination Setfachbereich on modify { create; update; }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
 }

field ( readonly : update ) zz_tech_fachbe;

}

