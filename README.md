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
   
* // Add explicit assignments by splitting the custom field using substring
    with positions as (
        select srvc.mandt,
               srvc.service_uuid,
               srvc.zz_additional_workareas,
               position(';' in srvc.zz_additional_workareas) as pos1,
               position(';' in srvc.zz_additional_workareas, pos1 + 1) as pos2,
               position(';' in srvc.zz_additional_workareas, pos2 + 1) as pos3
        from "/PLCE/R_PDSERVICE" as srvc
        where srvc.zz_additional_workareas is not null
        limit 1
    )
    select srvc.mandt,
           case when pos.pos1 > 0 then substring(srvc.zz_additional_workareas, 1, pos.pos1 - 1)
                else srvc.zz_additional_workareas end as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    cross join positions as pos
    where srvc.zz_additional_workareas is not null
      and (pos.pos1 > 0 or srvc.zz_additional_workareas <> '')
    union all
    select srvc.mandt,
           case when pos.pos2 > pos.pos1 + 1 then substring(srvc.zz_additional_workareas, pos.pos1 + 1, pos.pos2 - pos.pos1 - 1) end as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    cross join positions as pos
    where srvc.zz_additional_workareas is not null
      and pos.pos2 > pos.pos1 + 1
    union all
    select srvc.mandt,
           case when pos.pos3 > pos.pos2 + 1 then substring(srvc.zz_additional_workareas, pos.pos2 + 1, pos.pos3 - pos.pos2 - 1) end as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    cross join positions as pos
    where srvc.zz_additional_workareas is not null
      and pos.pos3 > pos.pos2 + 1
    union all
    select srvc.mandt,
           case when length(srvc.zz_additional_workareas) > pos.pos3 + 1 then substring(srvc.zz_additional_workareas, pos.pos3 + 1) end as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    cross join positions as pos
    where srvc.zz_additional_workareas is not null
      and length(srvc.zz_additional_workareas) > pos.pos3 + 1
   )
   where work_area is not null;
  endmethod.
ENDCLASS.
