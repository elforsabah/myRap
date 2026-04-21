<lv_service_criticality> = COND #( 
  WHEN lv_reactiontime IS NOT INITIAL                    THEN 1
  WHEN ls_wr_ewa_order_object-main_position IS NOT INITIAL THEN 3
  ELSE 0 ).
