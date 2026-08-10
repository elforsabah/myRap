  data:
    LDETAILTABLE_Z type ref to DATA,
    LMODES_Z       type standard table of /WATP/DWASTEMODE,
    LDEFMODE_Z     type /WATP/DWASTEMODE,
    LINDEXES_Z     type standard table of INT4,
    LINDEX_Z       type INT4,
    LITEM_Z        type ref to /WATP/SCSBO_CABAVVD.
  field-symbols:
    <TABLE_Z> type standard table,
    <ITEM_Z>  type /WATP/SCSBO_CABAVVD.

* Verfahren nur vorbelegen, solange es genau einen zulässigen Wert gibt
  select WASTEMODE from /WATP/TTWSTMODE
    into table LMODES_Z up to 2 rows.                "#EC CI_NOFIRST

  if LINES( LMODES_Z ) = 1.
    read table LMODES_Z into LDEFMODE_Z index 1.

    LDETAILTABLE_Z = GETDETAILTABLE( PAR_NAME = 'AVV' ).
    if LDETAILTABLE_Z is bound.
      assign LDETAILTABLE_Z->* to <TABLE_Z>.
      if <TABLE_Z> is assigned.

*       1. Betroffene Zeilen sammeln
        loop at <TABLE_Z> assigning <ITEM_Z>.
          if <ITEM_Z>-WASTEMODE   is initial
            and <ITEM_Z>-DETAIL_INDEX is not initial.
            append <ITEM_Z>-DETAIL_INDEX to LINDEXES_Z.
          endif.
        endloop.

*       2. Über die BO-Schnittstelle schreiben
        loop at LINDEXES_Z into LINDEX_Z.
          clear LITEM_Z.
          AVV_CHANGING(
            exporting
              PAR_DETAILINDEX = LINDEX_Z
            importing
              PAR_AVV_ITEM    = LITEM_Z ).
          if LITEM_Z is bound.
            LITEM_Z->WASTEMODE = LDEFMODE_Z.
            AVV_CHANGED( PAR_DETAILINDEX = LINDEX_Z ).
          endif.
        endloop.

      endif.
    endif.
  endif.
