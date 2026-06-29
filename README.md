<img width="1649" height="900" alt="image" src="https://github.com/user-attachments/assets/405f56ef-506e-4728-8913-59fc4636f1b4" />






FUNCTION /WATP/CMA_WASTE_AVV_SHLP_EXIT.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  TABLES
*"      SHLP_TAB TYPE  SHLP_DESCT
*"      RECORD_TAB STRUCTURE  SEAHLPRES
*"  CHANGING
*"     REFERENCE(SHLP) TYPE  SHLP_DESCR
*"     REFERENCE(CALLCONTROL) LIKE  DDSHF4CTRL STRUCTURE  DDSHF4CTRL
*"----------------------------------------------------------------------

  SEARCHHELP_CALL_BADI.

  if CALLCONTROL-STEP = 'SELECT'.
    delete SHLP-SELOPT where SHLPFIELD = 'KEY_REF'.
    call function '/WATP/ARB_WASTE_AVV_SHLP_EXIT'
      tables
        SHLP_TAB = SHLP_TAB
        RECORD_TAB = RECORD_TAB
      changing
        SHLP = SHLP
        CALLCONTROL = CALLCONTROL.
  endif.

ENDFUNCTION.





function /WATP/ARB_WASTE_AVV_SHLP_EXIT.
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
      LWHERECLAUSE type STRING.
    DATA_BO_RANGE SHMTX.
    NEW_BO_RANGE SHMTX MAKTX MAKT-MAKTX.
**    LX030L type X030L,
*    LFLD_TAB type table of DFIES,
*    LRESULTS type standard table of /WATP/SARB_WASTE_SHLP.
*
*  call function 'DDIF_NAMETAB_GET'
*    EXPORTING
*      TABNAME   = '/WATP/SARB_WASTE_SHLP'
**    IMPORTING
**      X030L_WA  = LX030L
*    TABLES
*      DFIES_TAB = LFLD_TAB
*    EXCEPTIONS
*      others    = 0.



    loop at SHLP-SELOPT reference into LSELOPTREF.
      case LSELOPTREF->SHLPFIELD.
        when 'MATNR'.
          LSELOPTREF->SHLPNAME = 'V_EWAEL_WASTE'.
        when 'AVVCODE'.
          LSELOPTREF->SHLPNAME = '/WATP/TAVVMARA'.
        when 'MIRROR_ENTRY'.
          LSELOPTREF->SHLPNAME = '/WATP/TAVVCATH'.
        when 'MAKTX'.
          LSELOPTREF->SHLPNAME = 'V_EWAEL_WASTE'.
          LSELOPTREF->LOW  = TO_UPPER( LSELOPTREF->LOW ).
          LSELOPTREF->HIGH = TO_UPPER( LSELOPTREF->HIGH ).
          insert LSELOPTREF->* into table LSELOPT.
          delete SHLP-SELOPT.
      endcase.
    endloop.

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

*  perform MAP_ITAB_TO_RECORD_TAB in program SAPLSDSD
*     tables LRESULTS
*     using
*       SHLP-FIELDDESCR
*       LFLD_TAB
*     changing
*       RECORD_TAB[].
*
*  CALLCONTROL-STEP = 'DISP'.

endfunction.
