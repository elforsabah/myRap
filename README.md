" -------------------------------------------------------------------------
        " CUSTOM SUCCESS MESSAGE 009: "Auftrag &1 wurde erfolgreich storniert"
        " -------------------------------------------------------------------------
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id       = 'Z_MSG_CL_SERVICE_EXT' " <-- Verify your message class name
                                            number   = '009'
                                            severity = if_abap_behv_message=>severity-success
                                            v1       = <ls_service>-ServiceId ) " <-- Replaces &1
                      ) TO reported-service.
