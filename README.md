extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{

mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
 }

field ( readonly : update  ) zz_tech_fachbe;

}

extension for projection implementation in class zbp_e_bp_c_pdmnlservicewr_ext unique;

extend behavior for Service
{

use action assignworkarea;

}



@Metadata.layer: #CUSTOMER
annotate entity /PLCE/C_PDMNLServiceWR
    with 
{

//  @UI.lineItem: [ { position: 190, importance: #HIGH },
//  {
//  type:#FOR_ACTION,
//  dataAction:'assignworkarea',
//  label: 'Change ServiceType',
//  iconUrl: 'sap-icon://wrench',
//  inline: true,
//  emphasized: true,
//  importance: #HIGH,
//  position: 100,
//  invocationGrouping: #CHANGE_SET}]
//  @UI.textArrangement: #TEXT_ONLY
//  @UI.fieldGroup: [{position: 50, qualifier: 'DefaultInformation' }]
  @EndUserText.label: 'Fachbereich'
   Fachbereich;
}


