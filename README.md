DATA ls_srvcst TYPE /plce/tpdsrvcst.
          CLEAR ls_srvcst.
          ls_srvcst-mandt         = sy-mandt.
          ls_srvcst-service_uuid  = ls_asgmt-ServiceUUID.
          ls_srvcst-zz_bms_status = 'STORNIERT'.
          INSERT /plce/tpdsrvcst FROM ls_srvcst.
