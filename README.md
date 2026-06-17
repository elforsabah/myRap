  method CREATE_NOTIFICATION.

    data:
      LTNOTIFICATIONS         type /PLCE/IF_NOTIFICATION_API=>TY_T_NOTIFICATION,
      LNOTIFICATION_PROCESSOR type ref to /PLCE/CL_BGP_PDNOTIFICATION,
      LUUID                   type SYSUUID_X16,
      LLOGHANDLE              type BALLOGHNDL,
      LLOG_EEWA               type ref to CL_EEWA_IMPL_BO_CALLBACK_MLOG,
      LLOG_PLCE               type ref to /PLCE/CL_APPLLOG_HELPER.

    if PAR_USER_ID is not initial.

****      Log handle
      if FCALLBACK is not initial and FCALLBACK is instance of CL_EEWA_IMPL_BO_CALLBACK_MLOG.
        LLOG_EEWA ?= FCALLBACK.
        LLOGHANDLE = LLOG_EEWA->MSGLOG->HANDLE.
        create object LLOG_PLCE exporting IV_LOG_HANDLE = LLOGHANDLE.
      endif.

      try.
          LUUID = CL_UUID_FACTORY=>CREATE_SYSTEM_UUID( )->CREATE_UUID_X16( ).

          insert value #(
            ID = LUUID
            TYPE_KEY = PAR_NOTIFKEY
            TYPE_VERSION = '0.0.5'
            PRIORITY = /PLCE/IF_NOTIFICATION_API=>GCS_PRIORITIES-NEUTRAL
            RECIPIENTS = value #( ( ID = PAR_USER_ID ) )
            PARAMETERS = value #( ( LANGUAGE = SY-LANGU PARAMETERS = value #( ( NAME = 'TourId' VALUE = |{ PAR_TOUR_ID alpha = out }| )  ) ) )
          )
          into table LTNOTIFICATIONS.

          create object LNOTIFICATION_PROCESSOR.
          LNOTIFICATION_PROCESSOR->NOTIFICATIONS = LTNOTIFICATIONS.
          /PLCE/CL_BGPROCESSING_HELPER=>SCHEDULE( I_BGUNIT = LNOTIFICATION_PROCESSOR ).

        catch CX_UUID_ERROR.
**        nop
        catch /plce/cx_bgp into data(lex_bgp).
        if llog_plce is not initial.
            LLOG_PLCE->ADD_EXCEPTION(
              IV_SEVERITY   = IF_BALI_CONSTANTS=>C_SEVERITY_WARNING
              IRO_EXCEPTION = lex_bgp
            ).
        else.
**        nop
        endif.
*            create object EXCEPTION.
*            EXCEPTION->PUT_SYMSG( ).

      endtry.
    endif.

  endmethod.
