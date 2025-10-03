extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
// Precheck validates input; action updates ExtCustom.zz_tech_fachbe and returns updated Service
}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
association _WorkAreaServicesExt ;// Expose the new association
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
 }

field ( readonly : update ) zz_tech_fachbe;

}
