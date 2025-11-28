" Loop over keys (though typically one call, so one entry)
    LOOP AT parameter ASSIGNING FIELD-SYMBOL(<fs_key>).
      " Access input parameters
      DATA(lv_check_mode) = <fs_key>-%param-checkmode.
      DATA(lv_json) = <fs_key>-%param-records.

      " Deserialize JSON to internal table
      /ui2/cl_json=>deserialize(
        EXPORTING
          json = lv_json
          pretty_name = /ui2/cl_json=>pretty_mode-camel_case
        CHANGING
          data = gt_all_data_first
      ).

    ENDLOOP.
