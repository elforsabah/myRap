CLASS zcl_wr_pd_workarea_func_ext DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC .
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

/* 
  Expects a CDS table function ZE_P_PDWorkAreaServiceFuncExt
  with parameter p_client : abap.clnt and return columns:
    mandt : abap.clnt, work_area : NVARCHAR, service_uuid : /plce/pdservice_uuid
*/

    RETURN
      WITH RECURSIVE base AS (
        SELECT mandt,
               service_uuid,
               zz_additional_workareas AS rest
          FROM "/PLCE/R_PDSERVICE"
         WHERE zz_additional_workareas IS NOT NULL
           AND mandt = :p_client
      ),
      split AS (
        /* first slice */
        SELECT mandt,
               service_uuid,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN rest
                    ELSE SUBSTR(rest, 1, INSTR(rest, ';') - 1)
               END AS part,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN NULL
                    ELSE SUBSTR(rest, INSTR(rest, ';') + 1)
               END AS rest
          FROM base
        UNION ALL
        /* subsequent slices */
        SELECT mandt,
               service_uuid,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN rest
                    ELSE SUBSTR(rest, 1, INSTR(rest, ';') - 1)
               END AS part,
               CASE WHEN INSTR(rest, ';') = 0
                    THEN NULL
                    ELSE SUBSTR(rest, INSTR(rest, ';') + 1)
               END AS rest
          FROM split
         WHERE rest IS NOT NULL AND rest <> ''
      )

      SELECT mandt, work_area, service_uuid
        FROM (
              /* standard computed assignments from existing function */
              SELECT mandt, work_area, service_uuid
                FROM "/PLCE/P_PDWORKAREASERVICEFUNC"( p_client => :p_client )

              UNION ALL

              /* custom additional work areas from ZZ_* field */
              SELECT mandt,
                     TRIM(part) AS work_area,
                     service_uuid
                FROM split
               WHERE part IS NOT NULL AND TRIM(part) <> ''
             );

  ENDMETHOD.

ENDCLASS.
