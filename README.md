  method ZZDO_CONFIRM_WDOIS.
    data:
      LDONE       type ABAP_BOOL,
      LCOUNT_SUCC type I,
      LCOUNT_FAIL type I,
      LCOUNT      type I,
      LTEXT       type STRING,
      LCHAR       type CHAR255.

    clear: LCOUNT, LCOUNT_SUCC, LCOUNT_FAIL.
    break ZWR_EEWA_AUTO_CONFIRM.
    loop at ET_ORDERITEMS reference into data(LORDERITEM).
      LCOUNT = LCOUNT + 1.
      try.
          ZZ_EEWA_MSGLOG_CLEAR( ).
          ZZ_EEWA_MSGLOG_PREPARE( EV_KEY = conv #( LORDERITEM->ORDERNR ) ). "Für das Protokoll ist der Schlüssel nur die ORDERNR (ohne ORDER_LAUFNR)
*------------------------------------------------
          LDONE = ZZDO_CONFIRM_WDOI( EV_KEY = LORDERITEM->KEY ).
          if LDONE = ABAP_TRUE.
            LCOUNT_SUCC = LCOUNT_SUCC + 1.
            ZZ_EEWA_MSGLOG_SAVE( ).
          else.
            LCOUNT = LCOUNT - 1.
          endif.
          COMMIT_WORK( PAR_KIND = /WATP/CL_MSGLOG=>CMSGTYP_WARNING ).
        catch /WATP/CX_BASE into data(LEX).
          if AV_DISABLE_MSGLOG is initial.
            "Überschrift für die Fehlermeldung im Protokoll
            create object EXCEPTION type ZCX_WR_WATP_AUTO
              exporting
                MSGTYPE    = /WATP/CL_MSGLOG=>CMSGTYP_ERROR
                TEXTID     = ZCX_WR_WATP_AUTO=>ZWR_WDOC_AUTO_EAP_ERROR
                T100_MSGV1 = |{ LORDERITEM->ORDERNR alpha = out }|
                T100_MSGV2 = conv #( LORDERITEM->ORDER_LAUFNR )
                T100_MSGV3 = conv #( LORDERITEM->POBJNR ).
            CALLBACK_REPORT(
              PAR_KIND      = /WATP/IF_BO_CALLBACK_BASE=>C_KIND_WARN
              PAR_EXCEPTION = EXCEPTION
            ).
            TA_LOG_EXCEPTION LEX.
          endif.
          ROLLBACK_WORK( ).
          LCOUNT_FAIL = LCOUNT_FAIL + 1.
          ZZ_EEWA_MSGLOG_SAVE( ).
          COMMIT_WORK( ).
      endtry.
      if AV_MAXROWS > 0 and LCOUNT >= AV_MAXROWS.
        exit.
      endif.
    endloop.

    if ( LCOUNT_SUCC is not initial or LCOUNT_FAIL is not initial ) and
       ( AV_DISABLE_MSGLOG is initial or AV_DISABLE_MSGLOG = 'S' ).
      write LCOUNT_SUCC to LCHAR. condense LCHAR. LTEXT = LCHAR.
      if LCOUNT_SUCC < 1.
        _ZZTA_LOG_WARN_TEXT ZCX_WR_WATP_AUTO ZWR_WDOC_AUTO_COUNT_OK_CON LTEXT.
      else.
        _ZZTA_LOG_HINT_TEXT ZCX_WR_WATP_AUTO ZWR_WDOC_AUTO_COUNT_OK_CON LTEXT.
      endif.

      if LCOUNT_FAIL is not initial.
        write LCOUNT_FAIL to LCHAR. condense LCHAR. LTEXT = LCHAR.
        _ZZTA_LOG_WARN_TEXT ZCX_WR_WATP_AUTO ZWR_WDOC_AUTO_COUNT_NOTOK_CON LTEXT.

      endif.
    endif.

  endmethod.




    method ZZDO_CONFIRM_WDOI.
    data:
      LSTAT type J_STATUS,
      LEX   type ref to /WATP/CX_BASE,
      LEEX  type ref to CX_EEWA_BASE.

    RV_DONE = ABAP_FALSE.

    try.
        AO_BO_WDORDERITEM ?= AO_EEWA_BOCACHE->GETBO(
*          PAR_CLASSNAME = 'ZCL_WR_EEWA_BO_WDORDERITEM'
          PAR_CLASSNAME =  CL_EEWA_BO_OBJTYPE=>CL_GETBOCLASSNAME( 'EWAORDERITEM' )
          PAR_KEY       = EV_KEY
          PAR_LOCK      = 'X'
        ).
        AO_BO_WDORDERITEM->SET_CALLBACK( PAR_INTERFACE = AO_EEWA_CALLBACK ).
        try.
            AO_BO_WDORDER ?= AO_BO_WDORDERITEM->GET_BOWDORDER( ). "Der Kopf WDORDER

            AO_BO_WDORDERITEM->GETRECENTSTATUS( importing PAR_STATUS = LSTAT ).
            if LSTAT eq 'IWA11' or "WDO Item:Created
               LSTAT eq 'IWA12' or "WDO Item:Output
               LSTAT eq 'IWA13'.   "WDO Item:Positive Confirmation

**              Standard (automatisch Zurückmelden)
**            X Automatisch Zurückmelden
**            F Fehler beim automatischen Zurückmelden
**            Z automatisches Zurückmelden erfolgt
              data(LAUTO_PROCESS) = AO_BO_WDORDERITEM->DATAREF->ZZAUTO_PROCESS.
              AO_BO_WDORDERITEM->DATAREF->ZZAUTO_PROCESS = ZZGC_WDOC_AUTO_PROCESS-ERROR_F.
              AO_EEWA_BOCACHE->SAVE_BO( PAR_BO = AO_BO_WDORDERITEM ).
              AO_EEWA_BOCACHE->COMMIT_WORK( ).

              AO_BO_WDORDERITEM->RELOAD_DATA( ).
              try.
                  ZZDO_CONFIRM_WEIGHBILLS( EV_KEY = AO_BO_WDORDERITEM->DATAREF->KEY EV_POBJNR = AO_BO_WDORDERITEM->DATAREF->POBJNR ).

                  if LSTAT eq 'IWA11' or "WDO Item:Created
                     LSTAT eq 'IWA12'."WDO Item:Output
                    AO_BO_WDORDERITEM->CHECK_BOOK_CONFIRM_POS( ). "PAR_FOLLOW = 'X'
                    AO_BO_WDORDERITEM->BOOK_CONFIRM_POS( ).       "PAR_FOLLOW = 'X'

                  elseif LSTAT eq 'IWA13'. "WDO Item:Positive Confirmation
                    AO_BO_WDORDERITEM->CHECK_BOOK_REVIEW( ).
                    AO_BO_WDORDERITEM->BOOK_REVIEW( ).
                  endif.

                catch CX_EEWA_BASE into data(LEEWA_EXCEPTION).
                  if "ZCL_WR_EEWA_MISC=>IS_EEWA_LOCKEXCEPTION( PAR_EXCEPTION = LEEWA_EXCEPTION ) or "?
                   ( SY-MSGID = 'MC' and SY-MSGNO = '601' ) or
                   ( LEEWA_EXCEPTION->T100_MSGID eq 'ME' and LEEWA_EXCEPTION->T100_MSGNO eq '006' ). " User block on PuCo.
                    ROLLBACK_WORK( ).
                    AO_BO_WDORDERITEM->RELOAD_DATA( ).
                    AO_BO_WDORDERITEM->DATAREF->ZZAUTO_PROCESS = LAUTO_PROCESS.
                    AO_EEWA_BOCACHE->SAVE_BO( PAR_BO = AO_BO_WDORDERITEM ).
                    COMMIT_WORK( ).
                  endif.

                  data(LWATP_EXCEPTION) = /WATP/CL_ISU_MISC=>CONVERT_EXCEPTION_TO_WATP( LEEWA_EXCEPTION ).
                  AO_EEWA_CALLBACK->IF_EEWA_PROTOCOL~REPORT(
                    PAR_KIND      = LEEWA_EXCEPTION->MSGTYPE
                    PAR_EXCEPTION = LEEWA_EXCEPTION
                  ).
                  raise exception LWATP_EXCEPTION.
              endtry.
              AO_BO_WDORDERITEM->DATAREF->ZZAUTO_PROCESS = ZZGC_WDOC_AUTO_PROCESS-DONE_Z.
              AO_EEWA_BOCACHE->SAVE_BO( PAR_BO = AO_BO_WDORDERITEM ).
              AO_EEWA_BOCACHE->UNLOCK_BO( exporting PAR_BO = AO_BO_WDORDERITEM ).
              RV_DONE = ABAP_TRUE.
            endif.
          cleanup.
            AO_EEWA_BOCACHE->UNLOCK_BO( exporting PAR_BO = AO_BO_WDORDERITEM ).
        endtry.
        AO_EEWA_BOCACHE->UNLOCK_BO( exporting PAR_BO = AO_BO_WDORDERITEM ).

      catch CX_EEWA_BASE into LEEX.
        LEX = /WATP/CL_ISU_MISC=>CONVERT_EXCEPTION_TO_WATP( LEEX ).
        AO_EEWA_CALLBACK->IF_EEWA_PROTOCOL~REPORT( PAR_KIND = LEEX->MSGTYPE PAR_EXCEPTION = LEEX ).
        raise exception LEX.
    endtry.

  endmethod.


  
