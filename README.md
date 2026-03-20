" C. SUCCESS MESSAGE 009: "Auftrag &1 wurde erfolgreich storniert"
        APPEND VALUE #( %tky = <ls_service>-%tky
                        %msg = new_message( id       = 'Z_MSG_SVR_TOUR_EXT' 
                                            number   = '009'
                                            severity = if_abap_behv_message=>severity-information
                                            v1       = |{ <ls_service>-serviceid ALPHA = OUT }| ) " <-- Removes leading zeros
                      ) TO reported-service.
