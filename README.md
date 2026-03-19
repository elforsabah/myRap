method DOCHECKCONFIRMPOS.
  data:
    LERROR         type CHAR1,
    LPATH          type STRING,
    LTEXT          type STRING,
    LFIELD         type ref to TEWARESULTZ,
    LS_EWACONFPRED type EWACONFPRED,
    LH_ACTUAL_DATE type SYDATUM,
    LV_DATE        type SYDATUM,
    LV_TIME        type SY-UZEIT,
    LFIELDS        type EWAPRESULTZ,
    LCONFTYPE      type CONFTYPE,
    LCONTTYPE_REC  type ref to TEWACONFTYPE,
    LCONTTYPE_RECS type T_TEWACONFTYPE,
    LEX            type ref to CX_EEWA_BASE.

  field-symbols:
    <LDATA> type ANY.

  include EEWA_CONST01.

*   Hinweis 1138529
  CHECK_HEADER_STATUS( ).
  CHECK_RESULTFIELDS( PAR_STEP = PAR_CURRENTSTEP ).

  get time.
  if FISDETAILBO = ABAP_FALSE.
    LH_ACTUAL_DATE = CL_EEWA_BO_WDORDER=>CL_GET_SERVICEDATE( PAR_ORDERNR = DATAREF->ORDERNR ).
  else.
    LH_ACTUAL_DATE = GET_BOWDORDER( )->GET_SERVICEDATE( ).
  endif.
  if LH_ACTUAL_DATE > SY-DATUM.
    RAISE_BO CX_EEWA_BO_WDOC WDOORDER_INVALID_DATE.
  endif.
  if DATAREF->ACTUAL_DATE is initial.
    DATAREF->ACTUAL_DATE = LH_ACTUAL_DATE.
  endif.

  try.
      CHECK_CONFTYPE( PAR_CURRENTSTEP = PAR_CURRENTSTEP ).
    catch CX_EEWA_BASE into LEX.
      "Detaillierte Check des Rückmeldehinweises:
      CALLBACK_REPORT(
        exporting
          PAR_KIND      = IF_EEWA_PROTOCOL=>C_KIND_WARN
          PAR_EXCEPTION = LEX
      ).
      "Reaktion auf den Check:
      RAISE_BO CX_EEWA_BO_ORDER WDOITEM_CT_CHECK_POSFAILED.
  endtry.

  "Prüfung der Entsorgungsanlage...nur für Fehler.
  "Der gleiche Aufruf mit Callback beim eigentlichen Buchen
  CL_EEWA_BO_WDPLANT=>CL_CHECK_USAGE(
    exporting
      PAR_WDPLANTNR      = DATAREF->WDPLANTNR
      PAR_BOCACHE        = GET_BOCACHE( )
*      PAR_CALLBACK       = GET_CALLBACK( )
      PAR_OBJECTTYPE     = COT_WDORDERITEM
      PAR_PROC_OPERATION = CL_EEWA_BO_WDPLANT=>COP_FINALIZE
      PAR_KEYDATE        = DATAREF->ORDER_DATE "richtige Datum?
  ).

  DOHANDLESTEPCHECK( PAR_STEP = CCNFSTEP_CONFIRMPOS PAR_CURRENTSTEP = PAR_CURRENTSTEP ).
  if PAR_CURRENTSTEP = CCNFSTEP_CONFIRMPOS or PAR_CURRENTSTEP is initial. " BRF Call only if no check after prior process -> we cannot pass that information currently
    BRFCALL( PAR_EVENT = EVNT_BRF_ITEM_CONF ).
  endif.

*   check if placement date is valid EHP4
  if DATAREF->ACTUAL_DATE is not initial.
    if DATAREF->ACTUAL_TIME is not initial.
      LV_TIME = DATAREF->ACTUAL_TIME.
    else.
      LV_TIME = DATAREF->PLANNED_TIME.
    endif.
    LV_DATE = DATAREF->ACTUAL_DATE.
  else.
    LV_TIME = DATAREF->PLANNED_TIME.
    LV_DATE = DATAREF->ORDER_DATE.
  endif.

*   Changed MKahl (C5095353) If the item is not chained (e.g. customizing)
*   do not do anything
  if CL_EEWA_DCFL=>ISWDOITEMCHAIN( PAR_POS = DATAREF->EWA_ORDER_OBJECT ) is not initial.
    call method DCFL_OBJ->LINKAGE_TIME_CHECK
      exporting
        IV_ACTUALTIME       = LV_TIME
        IV_ACTUALDATE       = LV_DATE
        IV_ORDER_DATE_SUC   = DATAREF->ORDER_DATE_SUC
        IV_PLANNED_TIME_SUC = DATAREF->PLANNED_TIME_SUC
      exceptions
        NOT_VALID           = 1
        others              = 2.

    if SY-SUBRC <> 0 and DATAREF->SERNR_RM_NEW is initial.
      RAISE_BO CX_EEWA_BO_WDOC WDOITEM_NOT_VALID_DATE.
    endif.

*     check if predessor is confirmed
    call method ME->GET_EWACONFPRED
      exporting
        IV_BEH_TYPE     = DATAREF->BEH_TYPE
        IV_SERVICE_TYPE = DATAREF->SERVICE_TYPE
      receiving
        RS_EWACONFPRED  = LS_EWACONFPRED.

*     check status
    if LS_EWACONFPRED-CONFMODE is initial or
       LS_EWACONFPRED-CONFMODE ne '4'.
      call method DCFL_OBJ->PREDECESSOR_STATUS_CHECK
        exceptions
          PREDECESSOR_NOT_CONFIRNMED = 1
          others                     = 2.

      if SY-SUBRC <> 0.
        CL_EEWA_MISC=>RAISE_SYMSG( ).
      endif.

    endif.

*     check if regular WDO is in timeframe.
    call method CL_EEWA_DCFL=>WDO_NEIGHBOUR_CHECK
      exporting
        IS_ORDER_OBJECT  = DATAREF->*
      exceptions
        NOT_INSTALLED    = 1
        NOT_IN_TIMEFRAME = 2
        others           = 3.

    if SY-SUBRC <> 0.
      CL_EEWA_MISC=>RAISE_SYMSG( ).
    endif.
  endif.
endmethod.


method CHECK_RESULTFIELDS.
  data:
    LERROR    type        CHAR1,
    LPATH     type        STRING,
    LTEXT     type        STRING,
    LFIELD    type ref to TEWARESULTZ,
    LFIELDS   type        EWAPRESULTZ,
    LFILLUPFIELDS TYPE EWAPDDICFIELDNAMES,
    LINITIAL  type        ABAP_BOOL value ABAP_FALSE,
    LWGHDATAS type        EWAPWDOITEMWGHSUM_WEIGHACTIONS.

  field-symbols:
    <LDATA> type any.

* Check Required Result Fields
  LFIELDS = GETRESULTFIELDS( ).
  delete LFIELDS where RESREQ is initial.
  if SUPPRESS_REQVERIFY_EMPTYDRIVE( DATAREF->* ). "C5122687 suppress required field check for empty drive
    delete LFIELDS where SUPPR_OBLIG_CHECK is not initial.
  endif.
  CL_EEWA_ORDER_FILLUP_RESULT=>CL_GET_FILLUPFIELDS( exporting PAR_BO_ITEM = ME PAR_AREA = IF_EEWA_WDOC_FILLUPMETHOD=>C_AREA-ORDERITEM PAR_STEP = PAR_STEP changing PAR_FILLUPFIELDS = LFILLUPFIELDS ).
  loop at LFIELDS reference into LFIELD.
    read table LFILLUPFIELDS transporting no fields with key table_line = LFIELD->RESFLD binary search.
    if SY-SUBRC is initial.
      continue.
    endif.
    concatenate 'DATAREF->' LFIELD->RESFLD into LPATH.
    assign (LPATH) to <LDATA>.
    if <LDATA> is initial.

      if LFIELD->RESFLD = 'WASTE_WEIGHT' or LFIELD->RESFLD = 'WASTE_VOL'.
*       C5095353 -> Check Performance-Impact
        GET_SUM_WEIGHT_DATA( importing PAR_DATAS = LWGHDATAS ).
        if LFIELD->RESFLD = 'WASTE_WEIGHT'.
          loop at LWGHDATAS transporting no fields where NET_WEIGHT > 0.
            exit.
          endloop.
          if SY-SUBRC is not initial.
            LINITIAL = ABAP_TRUE.
          endif.
        else.
          loop at LWGHDATAS transporting no fields where VOLUME > 0.
            exit.
          endloop.
          if SY-SUBRC is not initial.
            LINITIAL = ABAP_TRUE.
          endif.
        endif.
      else.
        LINITIAL = ABAP_TRUE.
      endif.
    endif.

    if LINITIAL = ABAP_TRUE.
      LTEXT = LFIELD->RESFLDTXT.
      BO_LOG_ERROR_TEXT CX_EEWA_BO_WDOC RESULTFIELD_REQUIRED LTEXT.
      LERROR = 'X'.
      LINITIAL = ABAP_FALSE.
    endif.
  endloop.
  if LERROR is not initial.
    RAISE_BO CX_EEWA_BO_WDOC RESULTFIELDS_REQUIRED.
  endif.
endmethod.



method CHECK_BOOK_CONFIRM_POS.
  BO_CHECK_BOOK_START_PAR CONFIRM_POS.
    if PAR_CURRENTSTEP = CCNFSTEP_CONFIRMPOS or PAR_CURRENTSTEP is initial.
      BO_SAPPROCESS_CHECK WA18.
      __CHECK_RECENT_STATUS3 WA18 IWA11 IWA12 IWA41. "PANG PAUS PPBL.
    endif.

    DOCHECKCONFIRMPOS( PAR_CURRENTSTEP = PAR_CURRENTSTEP ).
  BO_CHECK_BOOK_END_PAR CONFIRM_POS.

  if PAR_FOLLOW is not initial and CL_EEWA_BO_WDORDER=>CUSTOMIZING-WDOI_CNFRMPS_RVW is not initial.
    CHECK_BOOK_REVIEW( PAR_FOLLOW = PAR_FOLLOW PAR_CURRENTSTEP = PAR_CURRENTSTEP ).
  endif.
endmethod.



