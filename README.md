  method /PLCE/IF_BGPROCESSING_UNIT~EXECUTE.

    data:
      LPROCESSOR type ref to /PLCP/CL_WA_CONFIRMATION,
      LEX        type ref to /PLCE/CX_BASEEXCEPTION.

    try.
        create object LPROCESSOR.
        LPROCESSOR->CONFIRM_TOURS(
          exporting
            IV_PROFILE = PROFILE
            IT_KEYS    = KEYS
        ).
      catch /PLCE/CX_BASEEXCEPTION into LEX.
        raise exception type /PLCE/CX_BGP_UNIT
          exporting
            PREVIOUS = LEX.
    endtry.

  endmethod.
