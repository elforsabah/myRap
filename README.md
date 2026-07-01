*&---------------------------------------------------------------------*
*&  Include           /WATP/MACRO_SEARCHHELP                           *
*&---------------------------------------------------------------------*

define SEARCHHELP_CALL_BADI.
  /WATP/CL_SEARCHHELP=>BADI_ESH_IF_F4_MODIFY_STEP(
    changing
      PAR_SHLP_TAB    = SHLP_TAB[]
      PAR_RECORD_TAB  = RECORD_TAB[]
      PAR_SHLP        = SHLP
      PAR_CALLCONTROL = CALLCONTROL
  ).
end-of-definition.

*&1 Strukturname
define BEGIN_SEARCHHELP_SELECT.
  if CALLCONTROL-STEP = 'SELECT'.
    data:
      LFLD_TABNAME type TABNAME,
      LRESULTS type standard table of &1.
    LFLD_TABNAME = '&1'.
    DATA_BO_RANGE SHLP.
end-of-definition.

define SEARCHHELP_CONVERTRANGE.
  /WATP/CL_CONVERT=>CONVERT_DDSHSELOPS_TO_RANGES(
    PAR_SELOPS = SHLP-SELOPT
    PAR_RANGES = LRANGESHLPS->*
  ).
end-of-definition.

define END_SEARCHHELP_SELECT.
    call function 'F4UT_RESULTS_MAP'
      exporting
        SOURCE_STRUCTURE         = LFLD_TABNAME
        APPLY_RESTRICTIONS       = SPACE
      tables
        SHLP_TAB                 = SHLP_TAB
        RECORD_TAB               = RECORD_TAB
        SOURCE_TAB               = LRESULTS
      changing
        SHLP                     = SHLP
        CALLCONTROL              = CALLCONTROL.

    CALLCONTROL-STEP = 'DISP'.
*   Don't process STEP DISP additionally in this call
    return.
  endif.
end-of-definition.

define BEGIN_SEARCHHELP_DISP.
  if CALLCONTROL-STEP = 'DISP'.
end-of-definition.

define END_SEARCHHELP_DISP.
  endif.
end-of-definition.

define SEARCHHELP_OPTIMIZE.
* Spaltenbreite optimieren
  call function 'F4UT_OPTIMIZE_COLWIDTH'
    tables
      SHLP_TAB    = SHLP_TAB
      RECORD_TAB  = RECORD_TAB
    changing
      SHLP        = SHLP
      CALLCONTROL = CALLCONTROL.
end-of-definition.

define SEARCHHELP_DISP_OPTIMIZE.
  BEGIN_SEARCHHELP_DISP.
* Spaltenbreite optimieren
  CALLCONTROL-STEP = 'PRESEL1'.
  SEARCHHELP_OPTIMIZE.
  CALLCONTROL-STEP = 'DISP'.
  return.
  END_SEARCHHELP_DISP.
end-of-definition.

define END_SEARCHHELP_SELECT_OPTIMIZE.
    call function 'F4UT_RESULTS_MAP'
      exporting
        SOURCE_STRUCTURE         = LFLD_TABNAME
        APPLY_RESTRICTIONS       = SPACE
      tables
        SHLP_TAB                 = SHLP_TAB
        RECORD_TAB               = RECORD_TAB
        SOURCE_TAB               = LRESULTS
      changing
        SHLP                     = SHLP
        CALLCONTROL              = CALLCONTROL.
    CALLCONTROL-STEP = 'DISP'.
    SEARCHHELP_DISP_OPTIMIZE.
  endif.
end-of-definition.

define BEGIN_SEARCHHELP_PRESEL.
  data:
    __LPRESEL_FIELDDESCR type ref to DFIES.
  if CALLCONTROL-STEP = 'PRESEL'.
end-of-definition.

define END_SEARCHHELP_PRESEL.
    return.
  endif.
end-of-definition.

define SEARCHHELP_SUPPRESS_SELOPTS.
  " stolen from function 'F4UT_SUPPRESS_SELECT_OPTIONS'
  read table SHLP-FIELDDESCR reference into __LPRESEL_FIELDDESCR
    with key FIELDNAME = '&1'.
  if SY-SUBRC is initial.
    __LPRESEL_FIELDDESCR->MASK+9(1) = 'X'.
  endif.
end-of-definition.


method BADI_ESH_IF_F4_MODIFY_STEP.

  data: LR_BADI_ESH_F4 type ref to BADI_ESH_IF_F4_MODIFY_STEP.

* Call BAdI
  try.
      get badi LR_BADI_ESH_F4
        filters
          SHLPNAME = PAR_SHLP-SHLPNAME.
      call badi LR_BADI_ESH_F4->MODIFY_SEARCH_HELP
        changing
          CT_SHLP_TAB    = PAR_SHLP_TAB
          CT_RECORD_TAB  = PAR_RECORD_TAB
          CS_SHLP        = PAR_SHLP
          CS_CALLCONTROL = PAR_CALLCONTROL.
    catch CX_ROOT.                                          "#EC *
  endtry.

endmethod.
