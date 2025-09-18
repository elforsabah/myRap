METHOD setpointoforigin.
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
    FIELDS ( ReferenceInternalId ServiceUUID )  // Use CDS casing; add ServiceUUID for key
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services)
    FAILED DATA(lt_failed).

  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
    DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.
    SELECT SINGLE point_of_origin
      FROM zi_wr_ewa_order_object
      WHERE referenceid = @<ls_service>-ReferenceInternalId  // Fix casing if needed
      INTO @lv_zzpoint_origin_wdoi.

    IF sy-subrc = 0 AND lv_zzpoint_origin_wdoi IS NOT INITIAL.
      // Check if ExtCustom exists; read it first
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        FIELDS ( ServiceUUID )  // Minimal to check existence
        WITH VALUE #( ( ServiceUUID = <ls_service>-ServiceUUID ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
        // Create if missing (adapt from your ZZServiceCreateDetermination)
        DATA: lv_cid TYPE string VALUE '$abap_cid1_',
              lt_ext_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_ExtCustom.
        APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
        <ls_ext_create>-ServiceUUID = <ls_service>-ServiceUUID.  // %tky or key
        <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service
          CREATE BY \_ExtCustom
          AUTO FILL CID SET FIELDS WITH lt_ext_create
          MAPPED DATA(lmapped)
          FAILED DATA(lfailed)
          REPORTED DATA(lreported).
        // Handle errors if needed
      ENDIF.

      // Now update the field
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zzpoint_origin_wdoi )
        WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
                        zzpoint_origin_wdoi = lv_zzpoint_origin_wdoi ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).
    ENDIF.
  ENDLOOP.
ENDMETHOD.
