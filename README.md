1. SE16 → /WATP/TWDPLANAVV → Execute (no selection).
Look at AVVCODE 200301 and 150101. One facility each? → continue. Empty table? → tell me, the facility part dies.

2. SE24 → create ZCL_BO_CSCABNAVV, superclass /WATP/CL_BO_CSCABNAVV.

3. Redefine AVV_FILL_UP. Paste:


method AVV_FILL_UP.
  data:
    LDETAILTABLE type ref to DATA,
    LMODES       type standard table of /WATP/DWASTEMODE,
    LDEFMODE     type /WATP/DWASTEMODE,
    LPLANTS      type standard table of EWAEL_WDPLANTNR.
  field-symbols:
    <TABLE> type standard table,
    <ITEM>  type /WATP/SCSBO_CABAVVD.

  SUPER->AVV_FILL_UP( PAR_CABNAVVS = PAR_CABNAVVS ).

  select WASTEMODE from /WATP/TTWSTMODE
    into table LMODES up to 2 rows.                  "#EC CI_NOFIRST
  if LINES( LMODES ) = 1.
    read table LMODES into LDEFMODE index 1.
  endif.

  LDETAILTABLE = GETDETAILTABLE( PAR_NAME = 'AVV' ).
  assign LDETAILTABLE->* to <TABLE>.
  loop at <TABLE> assigning <ITEM>.
    check <ITEM>-WDPLANT is initial and <ITEM>-WASTEMODE is initial.

    <ITEM>-WASTEMODE = LDEFMODE.

    clear LPLANTS.
    select WDPLANTNR from /WATP/TWDPLANAVV
      into table LPLANTS up to 2 rows                 "#EC CI_NOFIRST
     where AVVCODE = <ITEM>-AVVCODE.
    if LINES( LPLANTS ) = 1.
      read table LPLANTS into <ITEM>-WDPLANT index 1.
    endif.

    if <ITEM>-WASTEMODE is not initial or <ITEM>-WDPLANT is not initial.
      AVV_CHANGED( PAR_DETAILINDEX = <ITEM>-DETAIL_INDEX ).
    endif.
  endloop.
endmethod.
4. Activate.

5. SM34 → /WATP/VCCSSDAVV → row MP100 → field Class Waste List = ZCL_BO_CSCABNAVV → Save.

6. Test: new contract on MP100, open the popup. Every row shows RE.

Works → go to 8.
Nothing changed → the field isn't a class slot. Go to 7.
7. (only if 6 failed) SE80 → /WATP/CL_BO_CSCABNAVV → method AVV_FILL_UP → Enhance → implicit enhancement at end of method → paste the same code without the SUPER->AVV_FILL_UP( ) line. Activate. Retest.

8. Open contract 40000275 → confirm its existing 7/RE rows are unchanged.

9. SE80 → include /WATP/LCS_SDWASTEINTERFACECI1 → search WASTEMODE → find the field catalog → implicit enhancement at end of that method:


loop at <fieldcat> assigning field-symbol(<LFCAT>)
     where FIELDNAME = 'WASTEMODE'.
  <LFCAT>-EDIT = ABAP_FALSE.
endloop.
10. Tell the business: RE is done; WDF is derived per EWC code, not fixed at 67, and stays manual wherever more than one facility accepts that code.

Start with step 1 and tell me what you see.



<img width="1328" height="1024" alt="image" src="https://github.com/user-attachments/assets/27ce3d04-83ac-456b-90c2-6ee9a03f31cc" />
