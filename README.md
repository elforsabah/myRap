METHOD set_point_of_origin.
  READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
    FIELDS ( ReferenceInternalId )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
    DATA lv_zzpoint_origin_wdoi TYPE zwr_point_origin_wdoi.
    SELECT SINGLE point_of_origin
      FROM zi_wr_ewa_order_object
      WHERE referenceid = @<ls_service>-ReferenceInternalId
      INTO @lv_zzpoint_origin_wdoi.

    IF sy-subrc = 0 AND lv_zzpoint_origin_wdoi IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
        UPDATE FIELDS ( ZZPOINT_ORIGIN_WDOI )
        WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
                        ZZPOINT_ORIGIN_WDOI = lv_zzpoint_origin_wdoi ) ).
    ENDIF.
  ENDLOOP.
ENDMETHOD.
