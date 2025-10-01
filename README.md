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
   
* // Add explicit assignments by splitting the custom field (adapt from HANA split techniques)
    select srvc.mandt,
           substr_regexpr( '[^;]+' in srvc.zz_additional_workareas occurrence gen.series_key ) as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    inner join series_generate_integer(1, 1, (length(regexp_replace(srvc.zz_additional_workareas, '[^;]', '')) + 1)) as gen
    on 1 = 1
    where srvc.zz_additional_workareas is not null
      and substr_regexpr( '[^;]+' in srvc.zz_additional_workareas occurrence gen.series_key ) <> ''
   
   );
  endmethod.
ENDCLASS.
