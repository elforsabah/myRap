" Derive tour BMS status from its services
    " SENT      = all services FREIGEGEBEN
    " PARTIAL   = some FREIGEGEBEN, some ERROR
    " ERROR     = all ERROR or no services sent
    DATA lv_count_ok    TYPE i VALUE 0.
    DATA lv_count_error TYPE i VALUE 0.

    LOOP AT lt_asgmts INTO DATA(ls_a_check)
      WHERE TourUUID = ls_tour-TourUUID.
      " Read service status from the extension we just wrote
      SELECT SINGLE zz_bms_status
        FROM /plce/tpdsrvcst
        WHERE service_uuid = @ls_a_check-ServiceUUID
        INTO @DATA(lv_svc_stat).
      IF lv_svc_stat = 'FREIGEGEBEN'.
        lv_count_ok    = lv_count_ok + 1.
      ELSEIF lv_svc_stat = 'ERROR'.
        lv_count_error = lv_count_error + 1.
      ENDIF.
    ENDLOOP.

    DATA(lv_bms_status) = COND /plce/char20(
      WHEN lv_count_error = 0 AND lv_count_ok > 0  THEN 'SENT'
      WHEN lv_count_error > 0 AND lv_count_ok > 0  THEN 'PARTIAL'
      ELSE                                               'ERROR' ).
