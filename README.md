 include EEWA_EL_MACRO_RANGE.
  data:
      LORDERNRS   type standard table of EORDERNR,
      LORDERDATAS type standard table of EWA_ORDER_HEAD,
      LEXPORTDATA type ISU_WA_ORDERDOWN_S_EROUTE,
      LLOGHANDLE  type BALLOGHNDL,
      LLOG        type ref to CL_EEWA_IMPL_BO_CALLBACK_MLOG.

    LORDERNRS = value #( for L in FCHANGED_DATAS ( L-ORDERNR ) ).

    if LINES( LORDERNRS ) is not initial.

      sort LORDERNRS.
      delete adjacent duplicates from LORDERNRS.
      sort FCHANGED_DATAS by KEY.

      DATA_BO_RANGE X.
      SET_BO_RANGE_KEYS X __KEYS LORDERNRS.

      CL_EEWA_OR_WDORDER=>CL_READ(
        exporting
          PAR_OBJTYPE = CL_EEWA_BO_WDORDER=>COT_WDORDER
          PAR_RANGES  = LRANGEXS
        importing
          PAR_TABLE   = LORDERDATAS
      ).
