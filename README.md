INTERFACE zif_ta_weighing.
  METHODS enable_raise_exception.
  METHODS set_commit_mode
    IMPORTING iv_commit_mode TYPE zcl_wr_waa_ta_selfweighing=>ty_commit_mode.
  METHODS set_process_data
    IMPORTING is_process TYPE zcl_wr_waa_ta_selfweighing=>sty_weighing_request.
  METHODS execute
    IMPORTING iv_operation TYPE zcl_wr_waa_ta_selfweighing=>ty_operation.
  METHODS get_result_data
    RETURNING VALUE(rs_result) TYPE zwr_swaa_selfweigh_result.
ENDINTERFACE.
