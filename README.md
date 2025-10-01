CLASS zcl_wr_pd_workarea_func_ext DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_amdp_marker_hdb .
    class-methods get_workareas_services_cus 
         for table function ZE_P_PDWorkAreaServiceFuncExt .
    PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_pd_workarea_func_ext IMPLEMENTATION.
   method get_workareas_services_cus
    by database function for hdb
        language sqlscript
        options read-ONLY
        using /plce/r_pdservice /PLCE/P_PDWORKAREASERVICEFUNC .
        
     return select distinct mandt, work_area, service_uuid
       from (
*    // Call the standard computed assignments (wrapper select)
    select mandt, work_area, service_uuid
    from "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )  as SRVC
    
     union all
    
*    // Add explicit assignments by splitting the custom field (adapt from HANA split techniques)
    select srvc.mandt,
           trim(occurrences_regexpr( '[^;]+' in srvc.zz_additional_workareas ) in srvc.zz_additional_workareas occurrence gen.series_key ) as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    inner join series_generate_integer(1, 1, (length(regexp_replace(srvc.zz_additional_workareas, '[^;]', '')) + 2)) as gen  // +2 for safety on count
    on 1 = 1
    where srvc.zz_additional_workareas is not null
      and trim(occurrences_regexpr( '[^;]+' in srvc.zz_additional_workareas ) in srvc.zz_additional_workareas occurrence gen.series_key ) <> ''

);
  endmethod.
ENDCLASS.


Description	Resource	Path	Location	Type
SQLSCRIPT: sql syntax error: incorrect syntax near "in"	ZCL_WR_PD_WORKAREA_FUNC_EXT (Class)	adt/classlib/classes/zcl_wr_pd_workarea_func_ext	line 32	ABAP Syntax Check Problem
