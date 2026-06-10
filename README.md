"---------------------------------------------------------------------
        " Write per-service BMS status directly on /plce/tpdsrvcst
        " MODIFY ENTITIES OF /PLCE/R_PDService is forbidden inside the
        " Tour behavior handler — direct SQL on the base table is the
        " correct pattern (same approach used throughout this method)
        " ZZ_BMS_STATUS must exist via append structure ZSRVCST_BMS_EXT
        "---------------------------------------------------------------------
        UPDATE /plce/tpdsrvcst
          SET zz_bms_status = @lv_svc_bms_status
          WHERE service_uuid = @ls_asgmt-ServiceUUID.

        IF sy-subrc <> 0.
          " No row exists yet for this service — insert one
          DATA ls_srvcst TYPE /plce/tpdsrvcst.
          CLEAR ls_srvcst.
          ls_srvcst-mandt         = sy-mandt.
          ls_srvcst-service_uuid  = ls_asgmt-ServiceUUID.
          ls_srvcst-zz_bms_status = lv_svc_bms_status.
          INSERT /plce/tpdsrvcst FROM ls_srvcst.
        ENDIF.
