. Redefine AVV_FILL_UP. Paste:


method AVV_FILL_UP.
  data:
    LDETAILTABLE type ref to DATA,
    LMODES       type standard table of /WATP/DWASTEMODE,
    LDEFMODE     type /WATP/DWASTEMODE.
  field-symbols:
    <TABLE> type standard table,
    <ITEM>  type /WATP/SCSBO_CABAVVD.

  SUPER->AVV_FILL_UP( PAR_CABNAVVS = PAR_CABNAVVS ).

* nur vorbelegen, solange es genau einen zulässigen Wert gibt
  select WASTEMODE from /WATP/TTWSTMODE
    into table LMODES up to 2 rows.                  "#EC CI_NOFIRST
  check LINES( LMODES ) = 1.
  read table LMODES into LDEFMODE index 1.

  LDETAILTABLE = GETDETAILTABLE( PAR_NAME = 'AVV' ).
  assign LDETAILTABLE->* to <TABLE>.
  loop at <TABLE> assigning <ITEM>.
    check <ITEM>-WASTEMODE is initial.
    <ITEM>-WASTEMODE = LDEFMODE.
    AVV_CHANGED( PAR_DETAILINDEX = <ITEM>-DETAIL_INDEX ).
  endloop.
endmethod.
4. Activate.

5. SM34 → /WATP/VCCSSDAVV → row MP100 → Class Waste List = ZCL_BO_CSCABNAVV → Save.

6. Test: new contract on MP100, open the popup. Every row shows RE.
Works → step 8. Nothing changed → step 7.

7. (only if 6 failed) SE80 → /WATP/CL_BO_CSCABNAVV → method AVV_FILL_UP → Enhance → implicit enhancement at end of method → same code without the SUPER-> line. Activate. Retest.

8. Open contract 40000275 → its existing 7/RE rows must be unchanged.

9. SE80 → include /WATP/LCS_SDWASTEINTERFACECI1 → search WASTEMODE → find the field catalog → implicit enhancement at end of that method:


loop at <fieldcat> assigning field-symbol(<LFCAT>)
     where FIELDNAME = 'WASTEMODE'.
  <LFCAT>-EDIT = ABAP_FALSE.
endloop.
