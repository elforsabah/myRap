Phase A — verify (read-only)
Step 1. Does the facility exist to be derived at all?
SE24 → /WATP/CL_BO_ARBNW → method READ_ARBNW. Note its exact signature (the code you sent calls it with PAR_RANGES, with older PAR_NWNR/PAR_NWINTNR params commented out) and note which table it selects from. Then SE16 on that table.

Rows exist for your EWC codes → both columns can be defaulted. Full scope.
No rows → only RE can be defaulted; WDPLANT stays manual. Say so to the business now — this still meets the written acceptance criterion, which only promises the procedure.
Step 2. Find the ALV builder.
SE11 → structure /WATP/SCS_CABAVVD_DP → Where-Used List → Programs + Classes. The hit that builds a field catalog over it is where the columns are made input-ready. Note the object name.

Step 3. Is "Class Waste List" an implementation-class slot?
SM34 → /WATP/VCCSSDAVV → the field is blank for MP100. Find its field name in the underlying view, then SE11 where-used on that field. You're looking for a dynamic instantiation (CREATE OBJECT … TYPE (…)) in /WATP/CL_BO_CSCABNAVV_BASE=>CL_GETCONFIG or /WATP/CL_CS_AVVCACHE_FACTORY.

Yes → Step 5a (clean, no enhancement).
No → Step 5b.
Step 4. Check DEFAULT_AVV.
SE11 → data element /WATP/DCS_CABN_DEFAULT (the one behind DEFAULT_AVV) → Where-Used → Programs/Classes. If the add-on already ships a defaulting mechanism, use it and drop Steps 5–6 entirely. Two minutes; worth it.

Phase B — implement
Step 5a. Subclass (if Step 3 said yes)

SE24 → create ZCL_BO_CSCABNAVV, superclass /WATP/CL_BO_CSCABNAVV, then redefine AVV_FILL_UP:


method AVV_FILL_UP.
  data:
    LDETAILTABLE type ref to DATA,
    LMODES       type standard table of /WATP/DWASTEMODE,
    LDEFMODE     type /WATP/DWASTEMODE,
    LARB_NW      type /WATP/SARBNWINFO.
  field-symbols:
    <TABLE> type standard table,
    <ITEM>  type /WATP/SCSBO_CABAVVD.

  SUPER->AVV_FILL_UP( PAR_CABNAVVS = PAR_CABNAVVS ).

* Verfahren nur vorbelegen, solange es genau einen zulässigen Wert gibt
  select WASTEMODE from /WATP/TTWSTMODE
    into table LMODES up to 2 rows.                 "#EC CI_NOFIRST
  if LINES( LMODES ) = 1.
    read table LMODES into LDEFMODE index 1.
  endif.

  LDETAILTABLE = GETDETAILTABLE( PAR_NAME = 'AVV' ).
  assign LDETAILTABLE->* to <TABLE>.
  loop at <TABLE> assigning <ITEM>.
    check <ITEM>-WDPLANT is initial and <ITEM>-WASTEMODE is initial.

*   Entsorgungsnachweis zum AVV-Code lesen -> Anlage + Verfahren
*   TODO Signatur laut Schritt 1 anpassen
    clear LARB_NW.
"   LARB_NW = /WATP/CL_BO_ARBNW=>READ_ARBNW( PAR_RANGES = ... ).

    if LARB_NW is not initial.
      <ITEM>-WDPLANT   = LARB_NW-WDPLANT.
      <ITEM>-WASTEMODE = LARB_NW-WASTEMODE.
      <ITEM>-NWNR      = LARB_NW-NWNR.
      <ITEM>-NWINTNR   = LARB_NW-NWINTNR.
    else.
      <ITEM>-WASTEMODE = LDEFMODE.
    endif.

    if <ITEM>-WASTEMODE is not initial or <ITEM>-WDPLANT is not initial.
      AVV_CHANGED( PAR_DETAILINDEX = <ITEM>-DETAIL_INDEX ).
    endif.
  endloop.
endmethod.
Then SM34 → /WATP/VCCSSDAVV → enter ZCL_BO_CSCABNAVV in Class Waste List for MP100 (and for SP_SUBSCRIPTION / SP_WASTE only if the business wants it there too — check first, they're separate service objects).

The check <ITEM>-WDPLANT is initial and <ITEM>-WASTEMODE is initial guard matters: it means a value a user has already typed is never overwritten, and re-running AVV_FILL_UP (it's called from PREPARE_INSERT, AVV_JOIN_UP and BOOK_AVV_FILL_UP) is idempotent.

Step 5b. Implicit enhancement (if Step 3 said no)

SE80 → /WATP/CL_BO_CSCABNAVV → method PREPARE_INSERT → Enhance → implicit enhancement point at end of method → paste the same body (without the SUPER-> line). Implicit enhancements do not require an object access key, so the partner namespace is not a blocker. The vendor's own comment — "Für Erweiterungszwecke überschrieben. Bitte stehen lassen!" — confirms this is the intended use of that method.

Step 6. Make the columns non-input.
In the object from Step 2, find the field catalog build and set EDIT = abap_false for WASTEMODE. If that object is also in /WATP/, use an implicit enhancement at the end of the catalog-building method and patch the entry there rather than modifying the loop.

Ask the business first whether they want WDPLANT locked too. The acceptance criterion only covers the procedure, and locking the facility removes the user's ability to correct a wrong derivation.

Phase C — test
Step 7. Create a new contract on MP100, open the popup.

Every row shows RE, greyed out.
Rows whose EWC code has an approval show the facility (150101 → 7, 200301 → 67).
Tick a row, save, reopen — values persisted.
SE16 on /WATP/TCSCABAVVD for the new approval key — WASTEMODE filled on the rows.
Step 8. Regression on the existing data: open contract 40000275 (approval 0000001233). The two already-maintained rows must still show 7/RE unchanged — that verifies the check … is initial guard.

What I'd do first
