function Z_ARB_WASTE_AVV_SHLP_EXIT.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  TABLES
*"      SHLP_TAB TYPE  SHLP_DESCT
*"      RECORD_TAB STRUCTURE  SEAHLPRES
*"  CHANGING
*"     REFERENCE(SHLP) TYPE  SHLP_DESCR
*"     REFERENCE(CALLCONTROL) LIKE  DDSHF4CTRL STRUCTURE  DDSHF4CTRL
*"----------------------------------------------------------------------

  BEGIN_SEARCHHELP_SELECT /WATP/SARB_WASTE_SHLP.
*  check CALLCONTROL-STEP = 'SELECT'.

    data:
      LSELOPTREF   type ref to DDSHSELOPT,
      LSELOPT      type table of DDSHSELOPT,
      LWHERECLAUSE type STRING,
      LV_VBELN     type VBELN,
      LV_POSNR     type POSNR_VA,
      LR_MATNR     type ZCL_WR_CCMG_MISC=>TT_MATNR_RNG.
    DATA_BO_RANGE SHMTX.
    NEW_BO_RANGE SHMTX MAKTX MAKT-MAKTX.

    loop at SHLP-SELOPT reference into LSELOPTREF.
      case LSELOPTREF->SHLPFIELD.
        when 'MATNR'.
          LSELOPTREF->SHLPNAME = 'V_EWAEL_WASTE'.
        when 'AVVCODE'.
          LSELOPTREF->SHLPNAME = '/WATP/TAVVMARA'.
        when 'MIRROR_ENTRY'.
          LSELOPTREF->SHLPNAME = '/WATP/TAVVCATH'.
        when 'VBELN'.
          LV_VBELN = LSELOPTREF->LOW.
        when 'POSNR'.
          LV_POSNR = LSELOPTREF->LOW.
        when 'MAKTX'.
          LSELOPTREF->SHLPNAME = 'V_EWAEL_WASTE'.
          LSELOPTREF->LOW  = TO_UPPER( LSELOPTREF->LOW ).
          LSELOPTREF->HIGH = TO_UPPER( LSELOPTREF->HIGH ).
          insert LSELOPTREF->* into table LSELOPT.
          delete SHLP-SELOPT.
      endcase.
    endloop.

*   keep the contract carriers out of the generic WHERE clause
    delete SHLP-SELOPT where SHLPFIELD = 'VBELN'.
    delete SHLP-SELOPT where SHLPFIELD = 'POSNR'.

    if LINES( LSELOPT ) > 0.
      /WATP/CL_CONVERT=>CONVERT_DDSHSELOPS_TO_RANGES(
        PAR_SELOPS = LSELOPT
        PAR_RANGES = LRANGESHMTXS->*
      ).
    endif.

    READ_BO_RANGE LRANGESHMTXS:
      MAKTX.

    call function 'F4_CONV_SELOPT_TO_WHERECLAUSE'
      exporting
        GEN_ALIAS_NAMES = 'X'
      importing
        WHERE_CLAUSE    = LWHERECLAUSE
      tables
        SELOPT_TAB      = SHLP-SELOPT.

    if LINES( <LMAKTXRANGE> ) > 0.
      if LWHERECLAUSE is INITIAL.
        LWHERECLAUSE = ' UPPER( V_EWAEL_WASTE~MAKTX ) in @<LMAKTXRANGE> ' ##NO_TEXT.
      else.
        LWHERECLAUSE = ' ' && LWHERECLAUSE && ' and ( UPPER( V_EWAEL_WASTE~MAKTX ) in @<LMAKTXRANGE> ) ' ##NO_TEXT.
      endif.
    endif.

*   restrict to the waste fractions configured (active) on the contract item
    if LV_VBELN is not initial.
      LR_MATNR = ZCL_WR_CCMG_MISC=>GET_CONTRACT_WASTE(
                   IV_VBELN = LV_VBELN
                   IV_POSNR = LV_POSNR ).
      if LR_MATNR is initial.
*       contract has no active waste -> force an empty result
        LR_MATNR = value #( ( SIGN = 'I' OPTION = 'EQ' LOW = '##########' ) ).
      endif.
      if LWHERECLAUSE is INITIAL.
        LWHERECLAUSE = ' V_EWAEL_WASTE~MATNR in @LR_MATNR ' ##NO_TEXT.
      else.
        LWHERECLAUSE = ' ' && LWHERECLAUSE && ' and ( V_EWAEL_WASTE~MATNR in @LR_MATNR ) ' ##NO_TEXT.
      endif.
    endif.

    "SECURITY: The variable LWHERECLAUSE is filled only by SAP's function 'F4_CONV_SELOPT_TO_WHERECLAUSE' above. So an SQL injection is not possible.
    select
        V_EWAEL_WASTE~MATNR  as MATNR,
        V_EWAEL_WASTE~MAKTX as MAKTX,
        /WATP/TAVVMARA~AVVCODE as AVVCODE,
        /WATP/TAVVCATHT~DESCRIPTION as DESCRIPTION,
        /WATP/TAVVCATH~LFDNR as LFDNR,
        /WATP/TAVVCATH~MIRROR_ENTRY as MIRROR_ENTRY
      from V_EWAEL_WASTE
      inner join /WATP/TAVVMARA on V_EWAEL_WASTE~MATNR = /WATP/TAVVMARA~MATNR
      inner join /WATP/TAVVCATH on /WATP/TAVVMARA~AVVCODE = /WATP/TAVVCATH~AVVCODE
             and /WATP/TAVVMARA~LFDNR = /WATP/TAVVCATH~LFDNR
             and /WATP/TAVVCATH~VALID_FROM <= @SY-DATUM and /WATP/TAVVCATH~VALID_TO >= @SY-DATUM
      left outer join /WATP/TAVVCATHT on /WATP/TAVVCATH~AVVCODE = /WATP/TAVVCATHT~AVVCODE
             and /WATP/TAVVCATH~LFDNR = /WATP/TAVVCATHT~LFDNR
             and /WATP/TAVVCATHT~SPRAS = @SY-LANGU
      where V_EWAEL_WASTE~SPRAS = @SY-LANGU and (LWHERECLAUSE)
      into corresponding fields of table @LRESULTS.

  END_SEARCHHELP_SELECT.

endfunction.
