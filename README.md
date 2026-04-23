METHOD populateextcustomfromorder.

  " =========================================================================
  " Step 1: Read all services that triggered the determination
  " =========================================================================
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service
    FIELDS ( referenceid serviceuuid referenceinternalid )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services)
    FAILED DATA(lt_failed).

  IF lt_services IS INITIAL.
    RETURN.
  ENDIF.

  " =========================================================================
  " Step 2: Prepare bulk operation tables
  " =========================================================================
  DATA: lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
        lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom.

  " =========================================================================
  " Step 3: Process each service
  " =========================================================================
  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

    " Get BOTH fields from order object in ONE query
    SELECT SINGLE zz_tech_fachbe, wdplantnr
      FROM zi_wr_ewa_order_object
      WHERE referenceid = @<ls_service>-referenceid
      INTO @DATA(ls_order_fields).

    IF sy-subrc <> 0.
      " No order object found, skip
      CLEAR ls_order_fields.
      CONTINUE.
    ENDIF.

    " Check if ExtCustom exists for this service
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      FIELDS ( serviceuuid wdplantnr zz_tech_fachbe )
      WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
      RESULT DATA(lt_extcustom).

    IF lt_extcustom IS INITIAL.
      " ---------------------------------------------------------------------
      " ExtCustom DOESN'T EXIST - create it with order object values
      " ---------------------------------------------------------------------
      DATA(lv_cid) = |$cid_extcust_{ <ls_service>-serviceuuid }$|.

      APPEND VALUE #(
        serviceuuid = <ls_service>-serviceuuid
        %target = VALUE #( (
          %cid = lv_cid
          zz_tech_fachbe = ls_order_fields-zz_tech_fachbe
          wdplantnr = ls_order_fields-wdplantnr
        ) )
      ) TO lt_ext_create.

    ELSE.
      " ---------------------------------------------------------------------
      " ExtCustom EXISTS - update only EMPTY fields
      " ---------------------------------------------------------------------
      DATA(ls_extcustom) = lt_extcustom[ 1 ].
      DATA(lv_needs_update) = abap_false.

      DATA: lv_new_fachbe TYPE ewa_order_object-zz_tech_fachbe,
            lv_new_wdplantnr TYPE ewael_wdplantnr.

      " Populate fachbereich if empty
      IF ls_extcustom-zz_tech_fachbe IS INITIAL 
         AND ls_order_fields-zz_tech_fachbe IS NOT INITIAL.
        lv_new_fachbe = ls_order_fields-zz_tech_fachbe.
        lv_needs_update = abap_true.
      ELSE.
        lv_new_fachbe = ls_extcustom-zz_tech_fachbe. " Keep existing
      ENDIF.

      " Populate wdplantnr if empty
      IF ls_extcustom-wdplantnr IS INITIAL 
         AND ls_order_fields-wdplantnr IS NOT INITIAL.
        lv_new_wdplantnr = ls_order_fields-wdplantnr.
        lv_needs_update = abap_true.
      ELSE.
        lv_new_wdplantnr = ls_extcustom-wdplantnr. " Keep existing
      ENDIF.

      " Only update if at least one field needs population
      IF lv_needs_update = abap_true.
        APPEND VALUE #(
          %key-serviceuuid = <ls_service>-serviceuuid
          zz_tech_fachbe = lv_new_fachbe
          wdplantnr = lv_new_wdplantnr
        ) TO lt_ext_update.
      ENDIF.
    ENDIF.

    CLEAR: ls_order_fields, lt_extcustom, ls_extcustom, lv_needs_update.

  ENDLOOP.

  " =========================================================================
  " Step 4: Bulk create missing ExtCustom records
  " =========================================================================
  IF lt_ext_create IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      CREATE BY \_extcustom
      AUTO FILL CID SET FIELDS WITH lt_ext_create
      MAPPED DATA(lmapped)
      FAILED DATA(lfailed)
      REPORTED DATA(lreported).
  ENDIF.

  " =========================================================================
  " Step 5: Bulk update empty fields
  " =========================================================================
  IF lt_ext_update IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      UPDATE FIELDS ( zz_tech_fachbe wdplantnr )
      WITH lt_ext_update
      FAILED DATA(lt_failed_update)
      REPORTED DATA(lt_reported_update).
  ENDIF.

ENDMETHOD.
