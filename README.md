FUNCTION ZZ_CMA_WASTE_AVV_SHLP_EXIT.
*"--------------------------------------------------------------------
*"*"Local Interface:
*"  TABLES
*"      SHLP_TAB TYPE  SHLP_DESCT
*"      RECORD_TAB STRUCTURE  SEAHLPRES
*"  CHANGING
*"     REFERENCE(SHLP) TYPE  SHLP_DESCR
*"     REFERENCE(CALLCONTROL) LIKE  DDSHF4CTRL STRUCTURE  DDSHF4CTRL
*"--------------------------------------------------------------------

*  SEARCHHELP_CALL_BADI.
*
*  if CALLCONTROL-STEP = 'SELECT'.
*    delete SHLP-SELOPT where SHLPFIELD = 'KEY_REF'.
*    call function '/WATP/ARB_WASTE_AVV_SHLP_EXIT'
*      tables
*        SHLP_TAB = SHLP_TAB
*        RECORD_TAB = RECORD_TAB
*      changing
*        SHLP = SHLP
*        CALLCONTROL = CALLCONTROL.
*  endif.


call function 'ZZ_ARB_WASTE_AVV_SHLP_EXIT'
  tables     SHLP_TAB = SHLP_TAB   RECORD_TAB = RECORD_TAB
  changing   SHLP = SHLP           CALLCONTROL = CALLCONTROL.



ENDFUNCTION.
