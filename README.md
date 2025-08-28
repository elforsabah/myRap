
        IF lv_tidnr <> ''.
         data(lv_tidnr_temp)  =  lv_tidnr.
         REPLACE all OCCURRENCES OF SPACE in lv_tidnr_temp with ''.
          IF lv_tidnr_temp <> ''.
           lv_html &&= |<br>{ lv_tidnr }|.
          ENDIF.
        ENDIF.
