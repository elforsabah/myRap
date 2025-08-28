| <b> { lTitle } </b><hr> { lTimeLine } <br> { lTaskTypeText }  <br> { lv_cus_name } <br> { lv_tidnr } <br> { lv_fraction } <br> { lv_image_data } <br> { lv_confnote_data } | ).


DATA(lv_html) = |<b>{ lTitle }</b><hr>{ lTimeLine }|.

IF lTaskTypeText IS NOT INITIAL.
  lv_html &&= |<br>{ lTaskTypeText }|.
ENDIF.

IF lv_cus_name IS NOT INITIAL.
  lv_html &&= |<br>{ lv_cus_name }|.
ENDIF.

IF lv_tidnr IS NOT INITIAL.
  lv_html &&= |<br>{ lv_tidnr }|.
ENDIF.

IF lv_fraction IS NOT INITIAL.
  lv_html &&= |<br>{ lv_fraction }|.
ENDIF.

IF lv_image_data IS NOT INITIAL.
  lv_html &&= |<br>{ lv_image_data }|.
ENDIF.

IF lv_confnote_data IS NOT INITIAL.
  lv_html &&= |<br>{ lv_confnote_data }|.
ENDIF.
