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
