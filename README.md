" =======================================================================
"  Main Position Criticality (Green for Grouped Services)
" =======================================================================
ASSIGN COMPONENT 'ZZ_MAIN_POSITION' OF STRUCTURE <ls_calculated> TO <lv_main_position>.
IF sy-subrc = 0.
  " Fill display field with converted number
  <lv_main_position> = ZCL_WR_EEWA_WDOC_MISC=>CL_CONVERT_OBJNR_OUT(
                         PAR_POBJNR = ls_wr_ewa_order_object-Main_position ).
ENDIF.

ASSIGN COMPONENT 'ZZ_MAIN_POS_CRITICALITY' OF STRUCTURE <ls_calculated> TO <lv_main_pos_crit>.
IF sy-subrc = 0.
  IF ls_wr_ewa_order_object-Main_position IS NOT INITIAL.
    " Belongs to a group (main or sub) -> Green
    <lv_main_pos_crit> = 3.
  ELSE.
    " No group -> Neutral
    <lv_main_pos_crit> = 0.
  ENDIF.
ENDIF.
