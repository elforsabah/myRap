CLASS zcl_wr_pd_workarea_func_ext DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.
  PUBLIC SECTION.
    INTERFACES if_amdp_marker_hdb.
    CLASS-METHODS get_workareas_services_cus
      FOR TABLE FUNCTION ZE_P_PDWorkAreaServiceFuncExt.
  PROTECTED SECTION.
  PRIVATE SECTION.
ENDCLASS.

CLASS zcl_wr_pd_workarea_func_ext IMPLEMENTATION.

  METHOD get_workareas_services_cus
    BY DATABASE FUNCTION FOR HDB
    LANGUAGE SQLSCRIPT
    USING "/PLCE/R_PDSERVICE" "/PLCE/P_PDWORKAREASERVICEFUNC".

    RETURN
      WITH base AS (
        SELECT mandt,
               service_uuid,
               zz_additional_workareas AS rest
          FROM "/PLCE/R_PDSERVICE"
         WHERE zz_additional_workareas IS NOT NULL
           AND mandt = :p_client
      ),
      split (mandt, service_uuid, part, rest, lvl) AS (
        /* first chunk */
        SELECT mandt,
               service_uuid,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN rest
                    ELSE SUBSTRING(rest, 1, INSTR(rest, ';') - 1)
               END AS part,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN NULL
                    ELSE SUBSTRING(rest, INSTR(rest, ';') + 1)
               END AS rest,
               1 AS lvl
          FROM base
        UNION ALL
        /* subsequent chunks */
        SELECT mandt,
               service_uuid,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN rest
                    ELSE SUBSTRING(rest, 1, INSTR(rest, ';') - 1)
               END AS part,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN NULL
                    ELSE SUBSTRING(rest, INSTR(rest, ';') + 1)
               END AS rest,
               lvl + 1
          FROM split
         WHERE rest IS NOT NULL AND rest <> ''
      )

      SELECT mandt, work_area, service_uuid
        FROM (
              /* Standard assignments */
              SELECT mandt, work_area, service_uuid
                FROM "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )

              UNION ALL

              /* Additional semicolon-separated work areas */
              SELECT mandt,
                     TRIM(part) AS work_area,
                     service_uuid
                FROM split
               WHERE part IS NOT NULL AND TRIM(part) <> ''
             );

  ENDMETHOD.

ENDCLASS.
