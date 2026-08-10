Done already
✓ SE16 /WATP/TARBNW → empty
✓ SE16 /WATP/TWDPLANAVV → empty → No source for the facility. Only RE gets defaulted.
Continue here
3. SE24 → create ZCL_BO_CSCABNAVV

Superclass: /WATP/CL_BO_CSCABNAVV
Description: Abfallliste BO mit Vorbelegung (WDF / Entsorgungsverfahren)
Instantiation: Public
Own Z package + workbench transport
4. In the class → protected section → redefine AVV_FILL_UP

SE24: Methods tab → cursor on the greyed AVV_FILL_UP → Redefine
ADT: add methods AVV_FILL_UP redefinition. under protected section
5. Paste the implementation:


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
6. Activate.

7. SM34 → /WATP/VCCSSDAVV → row MP100 → field Class Waste List = ZCL_BO_CSCABNAVV → Save (customizing transport).

8. Test: new contract on MP100 → open the popup.

Every row shows RE → go to 10
Nothing changed → go to 9
9. (only if 8 failed) SE80 → /WATP/CL_BO_CSCABNAVV → method AVV_FILL_UP → Enhance → implicit enhancement point at end of method → paste the same code without the SUPER->AVV_FILL_UP( ) line → activate → retest.

10. Regression: open contract 40000275 → its existing 7/RE rows must be unchanged.

11. Make the column non-input: SE80 → include /WATP/LCS_SDWASTEINTERFACECI1 → search WASTEMODE → find the field catalog build → implicit enhancement at end of that method:


loop at <fieldcat> assigning field-symbol(<LFCAT>)
     where FIELDNAME = 'WASTEMODE'.
  <LFCAT>-EDIT = ABAP_FALSE.
endloop.
12. Tell the business:

„RE" wird vorbelegt und ist nicht mehr eingabebereit — Akzeptanzkriterium erfüllt.
Der WDF kann nicht vorbelegt werden: er ist AVV-abhängig (150101 → 7, 200301 → 67) und es gibt im System keine Datenquelle — /WATP/TARBNW und /WATP/TWDPLANAVV sind beide leer. Wenn der WDF automatisch kommen soll, muss /WATP/TWDPLANAVV gepflegt werden (Stammdaten, keine Entwicklung).

You're at step 3/4. Ping me when step 8 gives a result.
