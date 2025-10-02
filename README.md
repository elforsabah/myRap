 modify entities of /PLCE/R_PDService in local mode
          entity SERVICE
            update from corresponding #( LDATA-SRV_UPD )
            create by \_SERVICETASK set fields with LDATA-TSKS_CREA
