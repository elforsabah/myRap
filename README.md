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
        options read-only
        using /plce/r_pdservice /plce/r_pdfunctionallocation /plce/r_pdworkareaprofile /plce/r_pdworkareapostalcode /plce/tpdsrvcst /PLCE/P_PDWORKAREASERVICEFUNC .
       
     return select distinct mandt, work_area, service_uuid
       from (
* // Call the standard computed assignments (wrapper select)
    select mandt, work_area, service_uuid
    from "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )
   
     union all
   
* // Add explicit assignments using recursive CTE
    with recursive split_workareas as (
        select srvc.mandt,
               srvc.service_uuid,
               srvc.zz_additional_workareas as remaining,
               substring(srvc.zz_additional_workareas, 1, coalesce(nullif(position(';' in srvc.zz_additional_workareas), 0), length(srvc.zz_additional_workareas))) as work_area,
               position(';' in srvc.zz_additional_workareas) as pos
        from "/PLCE/R_PDSERVICE" as srvc
        where srvc.zz_additional_workareas is not null
        union all
        select srvc.mandt,
               srvc.service_uuid,
               substring(remaining, pos + 1) as remaining,
               substring(remaining, 1, coalesce(nullif(position(';' in remaining), 0), length(remaining))) as work_area,
               position(';' in remaining) as pos
        from split_workareas
        where pos > 0 and length(remaining) > pos
    )
    select mandt, work_area, service_uuid
    from split_workareas
    where work_area is not null and trim(work_area) <> ''
   );
  endmethod.
ENDCLASS.
