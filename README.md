IF sy-subrc <> 0.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '017'
                   severity = if_abap_behv_message=>severity-warning
                   v1       = ls_tour-tourid
                   v2       = ls_tour-tourtemplate )
        ) TO reported-tour.
        CLEAR lv_team.
      ENDIF.



      IF sy-subrc <> 0 OR lv_movement_type IS INITIAL.
          lv_movement_type = 'S'.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '018'
                     severity = if_abap_behv_message=>severity-warning
                     v1       = ls_svc-service_uuid
                     v2       = ls_svc-action
                     v3       = ls_tour-tourid )
          ) TO reported-tour.
        ENDIF.


        
