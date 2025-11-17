TRY.
        ZCL_WEIGHING_HANDLER=>cl_weighing(
          EXPORTING
            iv_contractid = <ls_key>-%param-Vbeln " Sales Document
            iv_wasteid = <ls_key>-%param-Loadtype "Waste = MATNR
* iv_licence_plate = " License plate number
            iv_simulation = abap_true
            iv_user = SY-UNAME " User Name in User Master Record
* iv_commit = abap_false
          IMPORTING
            iv_weighing_result = gs_ls_weighing_result " returning information from weighing process
        ).
      CATCH cx_eewa_base INTO DATA(lx_error).
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'  " Use your custom message class
                   number = '005'  " Assume a message number for weighing error
                   severity = if_abap_behv_message=>severity-error
                   v1 = lx_error->get_text( ) )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind to relevant field if needed
        ) TO reported-weighingbridge.
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-weighingbridge.
        CONTINUE.  " Skip further processing for this failed key
      ENDTRY.
