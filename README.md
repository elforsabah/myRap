FUNCTION zwr_bms_update_service.
*"----------------------------------------------------------------------
*"*"Remote-Enabled Module:
*"  IMPORTING
*"     VALUE(SERVICE_UUID) TYPE /PLCE/PDSERVICE_UUID
*"     VALUE(POBJNR) TYPE J_OBJNR OPTIONAL
*"     VALUE(ZZ_BMS_STATUS) TYPE ZBMS_STATUS
*"  EXPORTING
*"     VALUE(EV_SUBRC) TYPE SY-SUBRC
*"----------------------------------------------------------------------
*" Called with DESTINATION 'NONE' so it commits in its own LUW, independent
*" of the calling RAP transaction. Once BMS has accepted an order, that
*" fact is true regardless of what SAP does afterwards.
*"
*" All importing parameters must be PASS BY VALUE — remote-enabled modules
*" reject reference parameters at activation.
*"----------------------------------------------------------------------

  DATA ls_srvcst TYPE /plce/tpdsrvcst.

  CLEAR ev_subrc.

  IF service_uuid IS INITIAL.
    ev_subrc = 4.
    RETURN.
  ENDIF.

  " --- 1. service custom fields ----------------------------------------
  UPDATE /plce/tpdsrvcst
    SET zz_bms_status = @zz_bms_status
    WHERE service_uuid = @service_uuid.

  IF sy-subrc <> 0.
    " no row yet — create one
    CLEAR ls_srvcst.
    ls_srvcst-service_uuid  = service_uuid.
    ls_srvcst-zz_bms_status = zz_bms_status.

    INSERT /plce/tpdsrvcst FROM ls_srvcst.

    IF sy-subrc <> 0.
      " lost a race against a parallel insert — retry the update once
      UPDATE /plce/tpdsrvcst
        SET zz_bms_status = @zz_bms_status
        WHERE service_uuid = @service_uuid.

      IF sy-subrc <> 0.
        ROLLBACK WORK.
        ev_subrc = 8.
        RETURN.
      ENDIF.
    ENDIF.
  ENDIF.

  " --- 2. disposal order position, linked by POBJNR ---------------------
  " /PLCE/R_PDSERVICE-REFERENCEINTERNALID = EWA_ORDER_OBJECT-POBJNR.
  " A missing position does not fail the call — the service status is
  " still correct and worth keeping.
  IF pobjnr IS NOT INITIAL.
    UPDATE ewa_order_object
      SET zz_bms_status = @zz_bms_status
      WHERE pobjnr = @pobjnr.
  ENDIF.

  COMMIT WORK AND WAIT.
  ev_subrc = 0.

ENDFUNCTION.
