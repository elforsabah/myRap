" 6d Container array
        "    container_number_old / container_number_new
        "    mapped from PROLOGA TIDNR fields via /plce/tpdsrvwr JOIN
        "    When beh_type_new is initial the container type is unknown —
        "    a placeholder is sent so BMS min-1-element validation passes.
        "    ContainerTypeName and ContainerTypeNumber use the service action
        "    as a readable placeholder until real data is available.
        "    ContainerNumberNew is set to '0' for movement type S because
        "    BMS requires it and no TIDNR is known in this fallback path.
        DATA lt_containers TYPE tt_containers.
        CLEAR lt_containers.
        IF ls_ewa-beh_type_new IS NOT INITIAL.
          APPEND VALUE #(
            quantity              = ls_ewa-beh_anzahl
            movement_type         = lv_movement_type
            container_type_name   = ls_ewa-beh_type_new
            container_type_number = ls_ewa-beh_type_new
            container_number_old  = ls_svc-container_atloc_tidnr
            container_number_new  = ls_svc-container_new_tidnr
            customer_owned        = abap_false
          ) TO lt_containers.
        ELSEIF lv_movement_type IS NOT INITIAL.
          " Minimal fallback — container type not known from EWA record
          APPEND VALUE #(
            quantity              = 1
            movement_type         = lv_movement_type
            container_type_name   = condense( ls_svc-action )
            container_type_number = condense( ls_svc-action )
            container_number_old  = condense( ls_svc-container_atloc_tidnr )
            container_number_new  = COND #(
                                      WHEN ls_svc-container_new_tidnr IS NOT INITIAL
                                      THEN condense( ls_svc-container_new_tidnr )
                                      ELSE '0' )
            customer_owned        = abap_false
          ) TO lt_containers.
        ENDIF.
