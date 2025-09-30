Description	Resource	Path	Location	Type
"/PLCE/R_PDSERVICE" ist mandantenabhängig und schränkt den Zugriff auf einen Mandanten ein. Die rufende AMDP-Methode "GET_WORKAREAS_SERVICES_CUS" muss diese Einschränkung ebenfalls ermöglichen.	ZCL_WR_PD_WORKAREA_FUNC_EXT (Class)	/R14_440_hwsb10035_de/.adt/classlib/classes/zcl_wr_pd_workarea_func_ext	line 19	ABAP Syntax Check Problem
CLASS zcl_wr_pd_workarea_func_ext DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .

  PUBLIC SECTION.

    INTERFACES if_amdp_marker_hdb .
    class-methods get_workareas_services_cus for table function ZE_P_PDWorkAreaServiceFuncExt.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_pd_workarea_func_ext IMPLEMENTATION.
   method get_workareas_services_cus
    by database function for hdb
        language sqlscript
        options read-only
        using /plce/r_pdservice /plce/r_pdfunctionallocation /plce/r_pdworkareaprofile /plce/r_pdworkareapostalcode /plce/tpdsrvcst.
        
     return select distinct mandt, work_area, service_uuid
       from (
*    // Call the standard computed assignments (wrapper select)
    select mandt, work_area, service_uuid
    from "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )
    
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
