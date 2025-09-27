@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Result Work Area - Service'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #XL,
  dataClass: #TRANSACTIONAL
}
define view entity /PLCE/P_PDWorkAreaService
  as select from /PLCE/P_PDWorkAreaServiceFunc( p_client: $session.client )
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

@AccessControl.authorizationCheck: #NOT_REQUIRED
@ClientHandling.type: #CLIENT_DEPENDENT
@ClientHandling.algorithm: #SESSION_VARIABLE
@EndUserText.label: 'Work Area Service - Table Function'
define table function /PLCE/P_PDWorkAreaServiceFunc
  with parameters
    @Environment.systemField: #CLIENT
    p_client : abap.clnt
returns
{
  mandt        : abap.clnt;
  work_area    : /plce/pdwork_area;
  service_uuid : /plce/pdservice_uuid;
}
implemented by method
  /PLCE/CL_PD_WORKAREA_FUNC=>GET_WORKAREAS_SERVICES;

    method get_workareas_services
    by database function for hdb
        language sqlscript
        options read-only
        using /plce/r_pdservice /plce/r_pdfunctionallocation /plce/r_pdworkareaprofile /plce/r_pdworkareapostalcode.
*   "*" not work for like, only "%" is allowed
    return select distinct SRVC.MANDT, WAPC.WORKAREA AS WORK_AREA, SRVC.SERVICEUUID AS SERVICE_UUID
    FROM "/PLCE/R_PDSERVICE" as SRVC
    INNER JOIN "/PLCE/R_PDWORKAREAPROFILE" as WAPR on WAPR.profile like SRVC.profile
*    INNER JOIN "/PLCE/R_PDWORKAREAPROFILE" as WAPR on SRVC.profile like ( CASE when WAPR.profile is NULL then '%' else WAPR.profile end )
    INNER JOIN "/PLCE/R_PDFUNCTIONALLOCATION" as FL on FL.functionallocation = SRVC.functionallocation
    INNER JOIN "/PLCE/R_PDWORKAREAPOSTALCODE" as WAPC on WAPC.workarea = WAPR.workarea and FL.postalcode like WAPC.postalcodesql
*    INNER JOIN "/PLCE/R_PDWORKAREAPOSTALCODE" as WAPC on WAPC.workarea = WAPR.workarea and FL.postalcode like ( CASE when WAPC.postalcodesql is NULL then '%' else WAPC.postalcodesql end )
*    INNER JOIN "/PLCE/R_PDWORKAREA" as WA on WA.workarea = WAPR.workarea
    WHERE SRVC.MANDT = :P_CLIENT;
*    SESSION_CONTEXT('CDS_CLIENT');
  endmethod.


@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Work Area Postal Code'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
  serviceQuality: #C,
  sizeCategory: #S,
  dataClass: #MASTER
}
define view entity /PLCE/R_PDWorkAreaPostalCode
  as select from /plce/cpdwapc
  association to parent /PLCE/R_PDWorkArea as _WorkArea on $projection.WorkArea = _WorkArea.WorkArea
{
      @ObjectModel.foreignKey.association: '_WorkArea'
  key work_area                      as WorkArea,
  key postal_code                    as PostalCode,

      replace(postal_code, '*', '%') as PostalCodeSql,

      /*Associations*/
      _WorkArea
}
