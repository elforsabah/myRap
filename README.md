" Map 6: Driving Time in Minutes
      ASSIGN COMPONENT 'ZZ_DRIVING_TIME_MIN' OF STRUCTURE <ls_calc> TO FIELD-SYMBOL(<lv_set_drive_min>).
      IF sy-subrc = 0. <lv_set_drive_min> = lv_driving_min. ENDIF.
