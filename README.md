extension implementation in class zbp_e_bp_r_pdtour unique;

extend behavior for Tour
{

 static action  ( precheck )  createtour parameter ZAE_D_TOURTOURTEMPLATE_AB result [0..*] $self;
 action touranBMSfreigeben result [0..*] $self;
 action stornoBMSService parameter ZAE_D_ServiceStorno_AB result [1] $self;
 determination assign_earliest_to_latest_date on modify { create; update; }

}

extend behavior for ExtCustom
{

 mapping for /PLCE/TPDTOURCST  {
    zz_remaining_duration = zz_remaining_duration;
 }

field ( readonly : update ) zz_remaining_duration;
}
