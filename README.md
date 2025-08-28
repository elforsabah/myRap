ATA: lv_d TYPE d,
      lv_t TYPE t.

CONVERT TIME STAMP lv_date_time TIME ZONE sy-zonlo INTO DATE lv_d TIME lv_t.
APPEND |{ lv_d DATE = USER } { lv_t TIME = USER }| TO lt_parts_image.
