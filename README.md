DATA: lv_total     TYPE zwr_d_cs_container_inventory,
      lv_current   TYPE zwr_d_cs_current_inventory,
      lv_delivered TYPE zwr_d_cs_delivered_inventory,
      lv_changed   TYPE zwr_d_cs_changed_inventory,
      lv_removed   TYPE zwr_d_cs_removed_inventory,
      lv_dummy     TYPE zwr_d_cs_dummy_inventory,
      lv_safety    TYPE zwr_d_cs_safetystock.




zcl_wr_watp_cs_misc=>cl_calc_container_inventory(
  EXPORTING par_date                = sy-datum
            par_behtyp              = CONV #( <row>-containertype )
            par_dayfactor           = lt_dayfactor
  IMPORTING par_current_inventory   = lv_current
            par_delivered_inventory = lv_delivered
            par_changed_inventory   = lv_changed
            par_removed_inventory   = lv_removed
            par_dummy_inventory     = lv_dummy
            par_safetystock         = lv_safety
  RECEIVING par_return              = lv_total ).
