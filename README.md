" Write per-service BMS status
        DATA(lv_svc_bms_status) = COND string(
          WHEN lv_http_status = 200 OR lv_http_status = 201
          THEN 'FREIGEGEBEN'
          ELSE 'ERROR' ).

        " Update on existing record
        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service BY \_ExtCustom
            UPDATE FIELDS ( zz_bms_status )
            WITH VALUE #(
              ( ServiceUUID   = ls_asgmt-ServiceUUID
                zz_bms_status = lv_svc_bms_status ) )
          FAILED   DATA(lf_svc)
          REPORTED DATA(lr_svc).

        " Create if not yet existing
        IF lf_svc IS NOT INITIAL.
          MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
            ENTITY Service
              CREATE BY \_ExtCustom
                FIELDS ( zz_bms_status )
                WITH VALUE #(
                  ( ServiceUUID = ls_asgmt-ServiceUUID
                    %target = VALUE #(
                      ( zz_bms_status = lv_svc_bms_status ) ) ) )
            FAILED   DATA(lf_svc_c)
            REPORTED DATA(lr_svc_c).
        ENDIF.
