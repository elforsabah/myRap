IF lf_auth_mod IS NOT INITIAL.
          MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
            ENTITY tour
              CREATE BY \_extcustom
                FIELDS ( zz_bms_status )
                WITH VALUE #( ( %cid    = |BMS_AUTH_{ ls_tour-touruuid }|
                                %tky    = ls_tour-%tky
                                %target = VALUE #( (
                                  %cid          = |BMS_AUTH_TGT_{ ls_tour-touruuid }|
                                  zz_bms_status = 'ERROR' ) ) ) )
            FAILED DATA(lf_auth_crt) REPORTED DATA(lr_auth_crt).
        ENDIF.



        IF lf_mod IS NOT INITIAL.
        MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
          ENTITY tour
            CREATE BY \_extcustom
              FIELDS ( zz_bms_status )
              WITH VALUE #( ( %cid    = |BMS_STAT_{ ls_tour-touruuid }|
                              %tky    = ls_tour-%tky
                              %target = VALUE #( (
                                %cid          = |BMS_STAT_TGT_{ ls_tour-touruuid }|
                                zz_bms_status = lv_bms_status ) ) ) )
          FAILED DATA(lf_crt) REPORTED DATA(lr_crt).
      ENDIF.
