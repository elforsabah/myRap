@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Extended Result Work Area - Service'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
serviceQuality: #C,
sizeCategory: #XL,
dataClass: #TRANSACTIONAL
}
define  view entity ZE_P_PDWorkAreaServiceExt
as select from ZE_P_PDWorkAreaServiceFuncExt( p_client: $session.client )
//association to parent /PLCE/R_PDServiceExtCustom as _ExtCustom
//on $projection.ServiceUUID = _ExtCustom.ServiceUUID
{
@Consumption.valueHelpDefinition: [{
entity: {
name    : '/PLCE/C_PDWorkAreaUser_VH',
element : 'WorkArea'
},
label     : 'Work Area',
useForValidation: true
}]
key work_area    as WorkArea,
key service_uuid as ServiceUUID
//_ExtCustom  // To-parent association
}

@EndUserText.label: 'Table Function Extension'
@AccessControl.authorizationCheck: #NOT_REQUIRED
@ClientHandling.type: #CLIENT_DEPENDENT
@ClientHandling.algorithm: #SESSION_VARIABLE

define table function ZE_P_PDWorkAreaServiceFuncExt
with parameters
    @Environment.systemField: #CLIENT
    p_client : abap.clnt
returns
{
  mandt : abap.clnt;
  work_area : /plce/pdwork_area;
  service_uuid : /plce/pdservice_uuid;
}
implemented by method
  zcl_pd_workarea_funct_ext=>GET_WORKAREAS_SERVICES_EXT;  // New custom class/method

extend view entity /PLCE/R_PDServiceExtCustom with 
association[0..1] to ZE_P_PDWorkAreaServiceExt  as _WorkAreaServicesExt on $projection.ServiceUUID = _WorkAreaServicesExt.ServiceUUID
 {
/plce/tpdsrvcst.zz_tech_fachbe,
_WorkAreaServicesExt
}

extension for projection implementation in class zbp_e_bp_c_pdmnlservicewr_ext unique;

extend behavior for Service
{
use action assignworkarea;
use association _WorkAreaServicesExt;
}

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

Description	Resource	Path	Location	Type
"ZE_P_PDWORKAREASERVICEEXT" ist nicht Teil einer Kompositionshierarchie von CDS-Entitäten.	ZE_BP_R_PDMNLSERVICE_EXT (Behavior Definition)	.adt/wbobj2/bo/bdef/ze_bp_r_pdmnlservice_ext	line 409	ABAP Syntax Check Problem

