DATA: lv_vbeln   TYPE vbak-vbeln VALUE '0000123456',
      lv_status  TYPE vbak-zz_status.

lv_status = 'A'.  " New value

UPDATE vbak
  SET zz_status = @lv_status
  WHERE vbeln   = @lv_vbeln.

IF sy-subrc = 0.
  COMMIT WORK.  " or let the caller handle the LUW
ELSE.
  " 0 rows updated (order not found or some error)
ENDIF.
