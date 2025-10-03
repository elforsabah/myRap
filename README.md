method get_workareas_services_ext
by database function for hdb
    language sqlscript
    options read-only
    using /plce/r_pdservice /plce/r_pdfunctionallocation /plce/r_pdworkareaprofile /plce/r_pdworkareapostalcode /plce/tpdsrvcst.  // Add /plce/tpdsrvcst for custom field

return select distinct mandt, work_area, service_uuid
from (
    // Call the standard computed assignments (wrapper select)
    select mandt, work_area, service_uuid
    from "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )

    union all

    // Add explicit assignments by splitting the custom field (adapt from HANA split techniques)
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
