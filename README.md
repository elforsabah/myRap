 data(lv_date)  = |{ lv_date_time(8) DATE = USER }|.
          data(lv_time)  = |{ lv_date_time+8(6) Time = USER } |.
          APPEND |{ lv_date } { lv_time }| TO lt_parts_image.


          Format directive "DATE" cannot be applied to the embedded expression.
