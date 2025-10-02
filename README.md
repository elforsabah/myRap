METHOD assignworkarea.
  " Step 1: Read child instances (use ALL FIELDS or specific for efficiency)
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service BY \_ExtCustom
      ALL FIELDS  " Or FIELDS ( ServiceUUID zz_tech_fachbe ) if needed
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_extcustom_read)
    FAILED DATA(lt_failed_read)
    REPORTED DATA(lt_reported_read).

  " Merge reported/failed
  reported = CORRESPONDING #( DEEP lt_reported_read ).
  failed = CORRESPONDING #( DEEP lt_failed_read ).

  " Step 2: Prepare create/update tables
  DATA: lt_extcustom_create TYPE TABLE FOR CREATE /PLCE/R_PDService\\ExtCustom,
        lt_extcustom_update TYPE TABLE FOR UPDATE /PLCE/R_PDService\\ExtCustom.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
    READ TABLE lt_extcustom_read WITH KEY %tky = <ls_key>-%tky INTO DATA(ls_read).
    IF sy-subrc = 0.
      " Update existing
      APPEND VALUE #( %tky                = ls_read-%tky
                      zz_tech_fachbe      = <ls_key>-%param-WorkArea
                      %control-zz_tech_fachbe = if_abap_behv=>mk-on )
             TO lt_extcustom_update.
    ELSE.
      " Create if missing (0..1 composition, assume 1:1 key alignment)
      APPEND VALUE #( %cid                = |CID{ sy-tabix }|  " Unique CID
                      %tky                = <ls_key>-%tky  " Parent key
                      zz_tech_fachbe      = <ls_key>-%param-WorkArea
                      %control-zz_tech_fachbe = if_abap_behv=>mk-on )
             TO lt_extcustom_create.
    ENDIF.
  ENDLOOP.

  " Step 3: Batch MODIFY on child entity
  MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY ExtCustom
      CREATE FROM lt_extcustom_create
      UPDATE FROM lt_extcustom_update
    MAPPED DATA(lt_mapped)
    FAILED DATA(lt_failed_modify)
    REPORTED DATA(lt_reported_modify).

  " Merge failed/reported
  failed = CORRESPONDING #( DEEP lt_failed_modify ).
  reported = CORRESPONDING #( DEEP lt_reported_modify ).

  " Step 4: Set action result ($self = Service instances)
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_service_result).

  result = VALUE #( FOR ls_service IN lt_service_result ( %tky   = ls_service-%tky
                                                         %param = ls_service ) ).
ENDMETHOD.
