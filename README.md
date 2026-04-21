
      " =======================================================================
      "  Main Position Criticality (Green for Grouped Services)
      " =======================================================================
      ASSIGN COMPONENT 'ZZ_MAIN_POSITION' OF STRUCTURE <ls_original> TO <lv_main_position>.
      <lv_main_position> = ls_wr_ewa_order_object-Main_position.
      IF sy-subrc = 0.
        ASSIGN COMPONENT 'SERVICE_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_main_pos_crit>.
        IF sy-subrc = 0.
          " Check if main position is filled

          IF <lv_main_position> IS NOT INITIAL.
            " Service belongs to a group -> Green (Success)
            <lv_main_pos_crit> = 3.
          ELSE.
            " Service does not belong to a group -> Neutral
            <lv_main_pos_crit> = 0.
          ENDIF.
        ENDIF.
      ENDIF.
      ENDIF.
