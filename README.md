// Add explicit assignments by splitting the custom field (adapt from HANA split techniques)
    select srvc.mandt,
           trim(occurrences_regexpr( '[^;]+' in srvc.zz_additional_workareas ) in srvc.zz_additional_workareas occurrence gen.series_key ) as work_area,
           srvc.service_uuid
    from "/PLCE/R_PDSERVICE" as srvc
    inner join series_generate_integer(1, 1, (length(regexp_replace(srvc.zz_additional_workareas, '[^;]', '')) + 2)) as gen  // +2 for safety on count
    on 1 = 1
    where srvc.zz_additional_workareas is not null
      and trim(occurrences_regexpr( '[^;]+' in srvc.zz_additional_workareas ) in srvc.zz_additional_workareas occurrence gen.series_key ) <> ''
