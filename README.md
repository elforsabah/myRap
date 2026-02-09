LOOP AT failed_srv-service ASSIGNING FIELD-SYMBOL(<fs_service>).
          " Use CORRESPONDING to map TourUUID and %is_draft automatically
          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> ) ) TO failed-tour.

          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                              number   = '002'
                                              severity = if_abap_behv_message=>severity-error )
                       ) TO reported-tour.
        ENDLOOP.

        IF sy-subrc <> 0.
          APPEND VALUE #( %tky = CORRESPONDING #( <fs_key> )
                          %msg = new_message( id       = 'ZWR_TOURDOC_MESSAGES'
                                              number   = '003'
                                              severity = if_abap_behv_message=>severity-information )
                        ) TO reported-tour.
        ENDIF.
