METHOD assignworkarea.
  " Step 1: Read child to confirm existence and get %tky
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service BY \_ExtCustom
      FIELDS ( ServiceUUID ZZ_TECH_FACHBE )  // Uppercase
      WITH VALUE #( FOR ls_key IN keys ( ServiceUUID = ls_key-ServiceUUID ) )
    RESULT DATA(lt_extcustom_read).

  " Step 2: Collect updates (direct child type)
  DATA: lt_extcustom_update TYPE TABLE FOR UPDATE /PLCE/R_PDServiceExtCustom.  // Full child entity

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
    " Find matching read or create if missing
    READ TABLE lt_extcustom_read INTO DATA(ls_read) WITH KEY ServiceUUID = <ls_key>-ServiceUUID.
    IF sy-subrc = 0.
      APPEND VALUE #( %tky = ls_read-%tky  // Full key from read
                      ZZ_TECH_FACHBE = <ls_key>-%param-WorkArea
                      %control-ZZ_TECH_FACHBE = if_abap_behv=>mk-on )  // Force update
             TO lt_extcustom_update.
    ELSE.
      " Optional: CREATE if child missing (0..1)
      APPEND VALUE #( %cid = |{ <ls_key>-ServiceUUID }|  // Client ID for create
                      ServiceUUID = <ls_key>-ServiceUUID
                      ZZ_TECH_FACHBE = <ls_key>-%param-WorkArea
                      %control = VALUE #( ServiceUUID = if_abap_behv=>mk-on
                                          ZZ_TECH_FACHBE = if_abap_behv=>mk-on ) )
             TO lt_extcustom_update.  // But use separate CREATE table if mixing
    ENDIF.
  ENDLOOP.

  " Step 3: MODIFY in root context (path optional for direct child)
  MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY ExtCustom  // Direct on child node
      UPDATE FIELDS ( ZZ_TECH_FACHBE )
      WITH lt_extcustom_update
    FAILED DATA(lt_failed)
    REPORTED DATA(lt_reported).

  " Proceed with error mapping, READ, and result as before...
ENDMETHOD.
