  data:
    LDETAILTABLE_Z type ref to DATA,
    LMODES_Z       type standard table of /WATP/DWASTEMODE,
    LDEFMODE_Z     type /WATP/DWASTEMODE.
  field-symbols:
    <TABLE_Z> type standard table,
    <ITEM_Z>  type /WATP/SCSBO_CABAVVD.

  select WASTEMODE from /WATP/TTWSTMODE
    into table LMODES_Z up to 2 rows.                "#EC CI_NOFIRST
  if LINES( LMODES_Z ) = 1.
    read table LMODES_Z into LDEFMODE_Z index 1.

    LDETAILTABLE_Z = GETDETAILTABLE( PAR_NAME = 'AVV' ).
    assign LDETAILTABLE_Z->* to <TABLE_Z>.
    loop at <TABLE_Z> assigning <ITEM_Z>.
      check <ITEM_Z>-WASTEMODE is initial.
      <ITEM_Z>-WASTEMODE = LDEFMODE_Z.
      AVV_CHANGED( PAR_DETAILINDEX = <ITEM_Z>-DETAIL_INDEX ).
    endloop.
  endif.
