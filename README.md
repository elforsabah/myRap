Add this code in the CALCULATE method, inside the LOOP:
 
      " Calculate Main Position Criticality
      IF <fs_data>-zz_main_position IS NOT INITIAL.
        <fs_data>-zz_main_pos_criticality = 3.  " Green
      ELSE.
        <fs_data>-zz_main_pos_criticality = 0.  " Neutral
      ENDIF.
 
