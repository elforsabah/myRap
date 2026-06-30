method get_contract_waste.
  constants lc_char_wastes type atnam value 'Z_CHAR_WASTELIST_WASTES'.

  select single cuobj from vbap
    where vbeln = @iv_vbeln and posnr = @iv_posnr
    into @data(lv_cuobj).
  if sy-subrc <> 0 or lv_cuobj is initial.
    return.
  endif.

  data lt_config type table of /* CONFIGURATION line type from SE37 */ cuxt_conf_line.
  call function 'VC_I_GET_CONFIGURATION'
    exporting instance      = lv_cuobj
             language       = sy-langu
    tables   configuration  = lt_config
    exceptions others       = 4.
  if sy-subrc <> 0.
    return.
  endif.

  loop at lt_config into data(ls) where atnam = lc_char_wastes.
    append value #( sign = 'I' option = 'EQ' low = ls-atwrt ) to rt_matnr.  " ATWRT already MATNR-18
  endloop.
endmethod.
