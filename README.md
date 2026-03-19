FUNCTION z_wr_cancel_waste_order_rfc.
*"----------------------------------------------------------------------
*"*"Local Interface:
*"  IMPORTING
*"     VALUE(IV_POBJNR) TYPE  CHAR30
*"     VALUE(IV_REASON_PREDEFINED) TYPE  CHAR255
*"     VALUE(IV_REASON_TEXT) TYPE  STRING
*"  EXPORTING
*"     VALUE(ET_RETURN) TYPE  BAPIRET2_T
*"----------------------------------------------------------------------

  TRY.
      zcl_wr_waste_order_api=>cancel_waste_order_item(
        iv_pobjnr            = iv_pobjnr
        iv_reason_predefined = iv_reason_predefined
        iv_reason_text       = iv_reason_text
      ).
      
      " Crucial: Commit the isolated RFC session so the DB update is written!
      COMMIT WORK AND WAIT.

    CATCH cx_eewa_base INTO DATA(lx_eewa).
      ROLLBACK WORK.
      " Pass the error back so the RAP Fiori app can display it
      APPEND VALUE #( type    = 'E'
                      id      = lx_eewa->if_t100_message~t100key-msgid
                      number  = lx_eewa->if_t100_message~t100key-msgno
                      message = lx_eewa->get_text( ) ) TO et_return.
  ENDTRY.

ENDFUNCTION.
