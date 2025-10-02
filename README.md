METHOD assignworkarea.
  " Step 1: Read child to check existence (path read pulls %tky if exists).
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service BY \_ExtCustom
      FIELDS ( ServiceUUID )  " Minimal fields to check existence
      WITH VALUE #( FOR ls_key IN keys ( ServiceUUID = ls_key-ServiceUUID ) )
    RESULT DATA(lt_extcustom_read).

  " Step 2: Prepare CREATE for missing children and UPDATE for existing.
  DATA: lt_extcustom_create TYPE TABLE FOR CREATE /PLCE/R_PDService \\ ExtCustom,
        lt_extcustom_update TYPE TABLE FOR UPDATE /PLCE/R_PDService \\ ExtCustom.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
    " Check if child exists for this parent key.
    READ TABLE lt_extcustom_read INTO DATA(ls_read) WITH KEY ServiceUUID = <ls_key>-ServiceUUID.
    IF sy-subrc = 0.
      " Exists: Prepare UPDATE using child's %tky.
      APPEND VALUE #( %tky = ls_read-%tky  " Full transient key from read
                      ZZ_TECH_FACHBE = <ls_key>-%param-WorkArea
                      %control-ZZ_TECH_FACHBE = if_abap_behv=>mk-on )  " Control: force update
             TO lt_extcustom_update.
    ELSE.
      " Missing: Prepare CREATE associated to parent.
      APPEND VALUE #( %key = <ls_key>-%key  " Use parent's %key for association
                      ServiceUUID = <ls_key>-ServiceUUID
                      ZZ_TECH_FACHBE = <ls_key>-%param-WorkArea
                      %control = VALUE #( ServiceUUID = if_abap_behv=>mk-on
                                          ZZ_TECH_FACHBE = if_abap_behv=>mk-on ) )
             TO lt_extcustom_create.
    ENDIF.
  ENDLOOP.

  " Step 3: Single MODIFY with CREATE and UPDATE (path-based).
  MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
      CREATE BY \_ExtCustom
        WITH lt_extcustom_create
      UPDATE BY \_ExtCustom
        FIELDS ( ZZ_TECH_FACHBE )
        WITH lt_extcustom_update
    FAILED DATA(lt_failed)
    REPORTED DATA(lt_reported).

  " Step 4: Debug/Inspect lt_failed (add this temporarily for troubleshooting).
  LOOP AT lt_failed-ExtCustom INTO DATA(ls_failed_ext).
    " Example: Log or raise for inspection (remove after fix).
    APPEND VALUE #( %key = ls_failed_ext-%key
                    %msg = new_message( id = 'Z_MSG_CLASS' number = '002'  " Custom msg class
                                        severity = if_abap_behv_message=>severity-error
                                        v1 = |Update failed for ServiceUUID: { ls_failed_ext-%key-ServiceUUID } - Check 404 details| ) )
           TO reported-ExtCustom.
  ENDLOOP.
  IF lines( lt_failed-Service ) > 0 OR lines( lt_failed-ExtCustom ) > 0.
    " Optional: Add generic error if any failed.
    APPEND VALUE #( %msg = new_message( id = 'Z_MSG_CLASS' number = '003'
                                        severity = if_abap_behv_message=>severity-error
                                        v1 = |One or more updates failed (404: Entity not found)| ) )
           TO reported-Service.
  ENDIF.

  " Step 5: Map errors to action's reported/failed.
  reported = CORRESPONDING #( DEEP lt_reported ).
  failed   = CORRESPONDING #( DEEP lt_failed ).

  " Step 6: Read updated Service (incl. _ExtCustom) for result.
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_read)
    FAILED failed
    REPORTED reported.

  " Step 7: Map to result.
  result = VALUE #( FOR ls_read IN lt_read
                    WHERE ( NOT %key IS INITIAL )
                    ( %key = ls_read-%key
                      %data = ls_read-%data ) ).
ENDMETHOD.


   modify entities of /PLCE/R_PDService in local mode
          entity SERVICE
            update from corresponding #( LDATA-SRV_UPD )
            create by \_SERVICETASK set fields with LDATA-TSKS_CREA


  " Step 3: Single MODIFY with CREATE and UPDATE (path-based, using CORRESPONDING for UPDATE).
MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
ENTITY Service
CREATE BY _ExtCustom
WITH lt_extcustom_create
UPDATE BY _ExtCustom
FIELDS ( ZZ_TECH_FACHBE )
FROM CORRESPONDING #( lt_extcustom_update )
FAILED DATA(lt_failed)
REPORTED DATA(lt_reported).

*   " Step 3: Single MODIFY with CREATE and UPDATE (path-based, using CORRESPONDING for UPDATE).
        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service
            UPDATE FROM CORRESPONDING #( lt_extcustom_update )
            CREATE BY \_ExtCustom
              FIELDS ( zz_tech_fachbe ) WITH 'TEC'.
         
