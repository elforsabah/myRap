class-data GV_VBELN type VBELN.
class-data GV_POSNR type POSNR_VA.



method get_contract_waste.
  data(lv_vbeln) = conv vbeln( |{ iv_vbeln alpha = in }| ).
  data(lv_posnr) = conv posnr_va( |{ iv_posnr alpha = in }| ).

  select single cuobj from vbap
    where vbeln = @lv_vbeln and posnr = @lv_posnr
    into @data(lv_cuobj).
  if sy-subrc <> 0 or lv_cuobj is initial.
    return.
  endif.

  data lt_config type table of /* CONFIGURATION line type from SE37 */ cuxt_conf_line.
  call function 'VC_I_GET_CONFIGURATION'
    exporting instance = lv_cuobj language = sy-langu
    tables   configuration = lt_config exceptions others = 4.

  loop at lt_config into data(ls) where atnam = 'Z_CHAR_WASTELIST_WASTES'.
    append value #( sign = 'I' option = 'EQ' low = ls-atwrt ) to rt_matnr.
  endloop.
endmethod.
