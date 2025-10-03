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
association _WorkAreaServicesExt { with draft; }  // Expose the new association
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
 }

field ( readonly : update ) zz_tech_fachbe;

}

@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Extended Result Work Area - Service'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZE_P_PDWorkAreaServiceExt as select from ZE_P_PDWorkAreaServiceFuncExt( p_client: $session.client )
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
}


CLASS zcl_pd_workarea_funct_ext DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.
  interfaces if_amdp_marker_hdb.
    class-methods get_workareas_services_ext for table function ZE_P_PDWorkAreaServiceFuncExt.

  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_pd_workarea_funct_ext IMPLEMENTATION.

METHOD get_workareas_services_ext
 by database function for hdb
        language sqlscript
        options read-only
        using  /PLCE/P_PDWORKAREASERVICEFUNC.

        return select distinct mandt, work_area, service_uuid
    from (
*    // Call the standard computed assignments (wrapper select)
    select mandt, work_area, service_uuid
    from "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )
    );
ENDMETHOD.

ENDCLASS.

extend view entity /PLCE/R_PDServiceExtCustom with 
association [0..*] to ZE_P_PDWorkAreaServiceExt as _WorkAreaServicesExt
    on $projection.ServiceUUID = _WorkAreaServicesExt.ServiceUUID
{
/plce/tpdsrvcst.zz_tech_fachbe,
_WorkAreaServicesExt
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






