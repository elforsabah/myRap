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



method GET_V_ZZWASTE_DEF.
  clear: ZCL_WR_CCMG_MISC=>GV_VBELN, ZCL_WR_CCMG_MISC=>GV_POSNR.

  data(lr_current) = me->collection_wrapper->get_current( ).
  if lr_current is bound.
    ZCL_WR_CCMG_MISC=>GV_VBELN = lr_current->get_property_as_string( 'VBELN' ).
    ZCL_WR_CCMG_MISC=>GV_POSNR = lr_current->get_property_as_string( 'POSNR' ).
  endif.

  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = 'ZZ_HCMA_WASTE_AVVCODE'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value #(
                          ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' ) )
    IV_OUTPUT_MAPPING = value #(
                          ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF'   F4_ATTR = 'MATNR' )
                          ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.
