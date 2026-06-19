method ZZ_ADJUST_NODE_WNOTE.
  data:
    LS_ITEM         type EWAEL_TREEV_ITEM,
    LD_PROPVALUEREF type ref to EWAEL_PROPVALUE,
    LD_INFOREF      type ref to EWAEL_TREEV_NODE_INFO,
    LV_TEXT         type STRING,
    LT_DATA         type standard table of STY_TWEIGHDATA.

  loop at PAR_INFOS reference into LD_INFOREF.

    " Safety check: skip if NODEREF or FIELDS are not available
    check LD_INFOREF->NODEREF is bound.

    read table LD_INFOREF->NODEREF->FIELDS
      with key NAME = 'POBJNR'
      reference into LD_PROPVALUEREF.

    " Only query DB if we have a valid POBJNR
    check sy-subrc = 0
      and LD_PROPVALUEREF is bound
      and LD_PROPVALUEREF->VALUE is not initial.

    select
      EWA_ORDER_WEIGH~WASTE,
      MAKT~MAKTG
      into table @LT_DATA
      from EWA_ORDER_WEIGH
      inner join MAKT
              on MAKT~MATNR = EWA_ORDER_WEIGH~WASTE
      where EWA_ORDER_WEIGH~POBJNR = @LD_PROPVALUEREF->VALUE
        and MAKT~SPRAS = @SY-LANGU.

    " Skip tree item update if no material descriptions found
    check LT_DATA is not initial.

    loop at LT_DATA reference into data(LS_DATA).
      LS_DATA->WASTE = |{ LS_DATA->WASTE alpha = out }|.
    endloop.

    loop at PAR_ITEMS into LS_ITEM
      where NODE_KEY = LD_INFOREF->NODEREF->NODE_KEY
        and ITEM_NAME = 21.

      data(LTABIX) = SY-TABIX.

      " FIX: Check SY-SUBRC before dereferencing LS_DATA
      read table LT_DATA
        with key WASTE = LS_ITEM-TEXT
        reference into LS_DATA.

      if sy-subrc = 0 and LS_DATA is bound.
        " Only update text if we found a matching material description
        LS_ITEM-TEXT = |{ LS_ITEM-TEXT }, { LS_DATA->MAT_DESC }|.
      endif.

      LS_ITEM-CLASS     = 2.
      LS_ITEM-FONT      = 2.
      LS_ITEM-ALIGNMENT = 3.
      modify PAR_ITEMS from LS_ITEM index LTABIX.
      exit.

    endloop.

    clear LT_DATA.  " Reset for next node iteration

  endloop.
endmethod.
