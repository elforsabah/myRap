 try.
        create object LLOG_PLCE
          exporting
            IV_LOG_HANDLE = LLOGHANDLE.
      catch /PLCE/CX_BASEEXCEPTION.
        " Log not yet in DB — LLOG_PLCE stays initial, no crash
        clear LLOG_PLCE.
      endtry.
