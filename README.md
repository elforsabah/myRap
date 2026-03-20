" -----------------------------------------------------------------
        " TRIGGER BACKGROUND TASK HERE!
        " This is completely hidden from RAP and perfectly safe.
        " -----------------------------------------------------------------
        IF iv_serviceuuid IS NOT INITIAL.
          CALL FUNCTION 'Z_WR_DELAYED_HARD_DELETE' IN BACKGROUND TASK
            EXPORTING
              iv_serviceuuid = iv_serviceuuid.
        ENDIF.
        " -----------------------------------------------------------------
