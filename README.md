IF sy-subrc = 0.
          " Map the flat database fields into the RAP %key structure
          APPEND VALUE #( %key-TourUUID    = ls_unassign-touruuid
                          %key-ServiceUUID = ls_unassign-serviceuuid ) TO lt_unassign_keys.
        ENDIF.
