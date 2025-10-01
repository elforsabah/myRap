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

    declare lt_results table (
        mandt abap.clnt,
        work_area nvarchar(255),  -- Adjust length to match your work area type
        service_uuid /plce/pdservice_uuid
    );

    -- Insert standard computed assignments into the table variable
    insert into :lt_results
    select mandt, work_area, service_uuid
    from "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client );

    -- Loop over services with explicit work areas and split them
    for srvc in (select mandt, service_uuid, zz_additional_workareas
                 from "/PLCE/R_PDSERVICE"
                 where zz_additional_workareas is not null) do
        declare lv_remaining nvarchar(255) := :srvc.zz_additional_workareas;
        declare lv_pos integer := 0;
        declare lv_part nvarchar(255) := '';

        while length(:lv_remaining) > 0 do
            lv_pos := position(';' in :lv_remaining);
            if :lv_pos = 0 then
                lv_part := :lv_remaining;
                lv_remaining := '';
            else
                lv_part := substring(:lv_remaining, 1, :lv_pos - 1);
                lv_remaining := substring(:lv_remaining, :lv_pos + 1);
            end if;

            if trim(:lv_part) <> '' then
                insert into :lt_results values (
                    :srvc.mandt,
                    :lv_part,
                    :srvc.service_uuid
                );
            end if;
        end while;
    end for;

    -- Return distinct results
    return select distinct mandt, work_area, service_uuid
           from :lt_results;

  endmethod.
ENDCLASS.
