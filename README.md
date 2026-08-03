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



class /WATP/CL_OR_ARBNW definition
  public
  inheriting from /WATP/CL_OBJECTREADER
  create public .

public section.
*"* public components of class /WATP/CL_OR_ARBNW
*"* do not include other source files here!!!
protected section.
*"* protected components of class /WATP/CL_OR_ARBNW
*"* do not include other source files here!!!

  types:
    BEGIN OF TADDITIONAL_NW_RANGE,
      OBJTYPE   type /WATP/DOBJTYPE,
      READER    type /WATP/DOBJREADER,
      RANGENAME type CHAR30,
    end of TADDITIONAL_NW_RANGE .
  types:
    TADDITIONAL_NW_RANGE_LIST type standard table of TADDITIONAL_NW_RANGE .

  data FRESULT_IS_EMPTY type /WATP/DKENNZX .
  data FADDITIONAL_NW_RANGE_LIST type TADDITIONAL_NW_RANGE_LIST .
  data FNWALIASES type STRING .

  methods GET_RESPONSIBLE_NW_READER
    returning
      value(PAR_RESULT) type /WATP/SOBJREAD_KEY .

  methods AFTER_SELECT_DATA_SPEC
    redefinition .
  methods CONVERT_RANGES
    redefinition .
  methods INITIALIZE
    redefinition .
  methods SELECT_DATA_SPEC
    redefinition .
private section.
*"* private components of class /WATP/CL_OR_ARBNW
*"* do not include other source files here!!!
ENDCLASS.



CLASS /WATP/CL_OR_ARBNW IMPLEMENTATION.


method AFTER_SELECT_DATA_SPEC.
  SUPER->AFTER_SELECT_DATA_SPEC(
    CHANGING
      PAR_LIST = PAR_LIST
  ).
  if EXISTS_FIELD( PAR_FIELDNAME = '/WATP/SOBJSTATUS' ) is not initial.
    /WATP/CL_OBJSTATUSMGR=>UPDATE_OBJSTATLIST(
      exporting
        PAR_GROUPNAME = '/WATP/SOBJSTATUS'
        PAR_FIELDNAME = 'OBJNR'
      changing
        PAR_LIST = PAR_LIST
    ).
  endif.
  if EXISTS_FIELD( PAR_FIELDNAME = '/WATP/SOBJSTATUS_DATA_EXT' ) is not initial.
    /WATP/CL_OBJSTATUSMGR=>UPDATE_OBJSTATLIST(
      exporting
        PAR_GROUPNAME = '/WATP/SOBJSTATUS_DATA_EXT'
        PAR_FIELDNAME = 'OBJNR'
      changing
        PAR_LIST = PAR_LIST
    ).
  endif.
endmethod.


method CONVERT_RANGES.
  data:
    LT_ADRUNCANR  type standard table of /WATP/DADRUNCANR,
    LPROOFTYPES   type standard table of /WATP/SARB_PROOFTYPE_KEY.

  clear FRESULT_IS_EMPTY.

  SUPER->CONVERT_RANGES( PAR_RANGES = PAR_RANGES ).

  READ_BO_RANGE PAR_RANGES:
    ADRUNCANR,
    UNIDENT,
    WASTELEGISLATION,
    ATTRIBUTE.

  DATA_BO_RANGES X PAR_RANGES.

  if <LUNIDENTRANGE> is not initial.
    select ADRUNCANR into table LT_ADRUNCANR
      from /WATP/TADRUNCA
     where UNIDENT   in <LUNIDENTRANGE>
       and ADRUNCANR in <LADRUNCANRRANGE>.

    NEW_BO_RANGE X ADRUNCANR /WATP/DADRUNCANR.

    if LT_ADRUNCANR is not initial.
      ADD_BO_RANGE_KEYS X ADRUNCANR LT_ADRUNCANR.
    else.
      FRESULT_IS_EMPTY = ABAP_TRUE.
    endif.
  endif.

  if FRESULT_IS_EMPTY is initial and
     ( <LWASTELEGISLATIONRANGE> is not initial or
       <LATTRIBUTERANGE> is not initial ).
    OR_READ_TABLE_DEF ARB_PROOFTYPE PAR_RANGES LPROOFTYPES.

    NEW_BO_RANGE X PROOFTYPE /WATP/DPROOFTYPE.

    if LPROOFTYPES is not initial.
      ADD_BO_RANGE_KEYS X PROOFTYPE LPROOFTYPES.
    else.
      FRESULT_IS_EMPTY = ABAP_TRUE.
    endif.

  endif.
endmethod.


method GET_RESPONSIBLE_NW_READER.

  data:
    LADDITIONAL_RANGE type ref to TADDITIONAL_NW_RANGE,
    LRANGE type ref to /WATP/SRANGE.

  field-symbols:
    <LRANGE>    type standard table.

* will return:
* a) the responsible reader
* b) ARB_NW -> no special reader
* c) empty -> conflicted ranges, return set is empty

  if FRANGES is not initial.
    loop at FADDITIONAL_NW_RANGE_LIST reference into LADDITIONAL_RANGE.
      read table FRANGES->* reference into LRANGE
        with key NAME = LADDITIONAL_RANGE->RANGENAME.
      if SY-SUBRC is initial and LRANGE is not initial." and LRANGE->RANGE->* is not initial.
        assign LRANGE->RANGE->* to <LRANGE>.
        if <LRANGE> is not initial.
          if PAR_RESULT is initial.
            PAR_RESULT-OBJTYPE = LADDITIONAL_RANGE->OBJTYPE.
            PAR_RESULT-OBJREADER = LADDITIONAL_RANGE->READER.
          elseif PAR_RESULT-OBJREADER <> LADDITIONAL_RANGE->READER or
                 PAR_RESULT-OBJTYPE   <> LADDITIONAL_RANGE->OBJTYPE.
            " no ranges for two special NW possible --> no reader to use, return set is empty
            clear PAR_RESULT.
            return.
          endif.
        endif.
      endif.
    endloop.
  endif.

  if PAR_RESULT is initial." return us as default reader
    PAR_RESULT-OBJTYPE = FREADER->OBJTYPE.
    PAR_RESULT-OBJREADER = FREADER->OBJREADER.
  endif.
endmethod.


method INITIALIZE.
  SUPER->INITIALIZE( ).
  SETSUPPORTEDFEATURE( PAR_NAME = C_FEATURE_FILL_OBJTYPE PAR_SUPPORTED = 'X' ).

  if FREADER->OBJTYPE = 'ARB_NW'.

    __ADD_ADDITIONAL_RANGE:
* NL
        PARTNER_ABS       ARB_NW_NL ARB_NW_NL,
        PARTNER_SCT       ARB_NW_NL ARB_NW_NL,
        ROUTE_COLLECT     ARB_NW_NL ARB_NW_NL,
        SPECIAL_RULES     ARB_NW_NL ARB_NW_NL,
        ADDRNUMBER_ABS    ARB_NW_NL ARB_NW_NL,
        ADDRNUMBER_SCT    ARB_NW_NL ARB_NW_NL,
        KUNNR             ARB_NW_NL ARB_NW_NL,
        ADDRNUMBER_KUNNR  ARB_NW_NL ARB_NW_NL,
        EXPORTED          ARB_NW_NL ARB_NW_NL,
* BE
        COLLECTION_ROUTE  ARB_NW_BE ARB_NW_BE,
        DIRECT_DELIVERY   ARB_NW_BE ARB_NW_BE,
        PARTNERCOL        ARB_NW_BE ARB_NW_BE,
        IDENTCOL          ARB_NW_BE ARB_NW_BE,
        IDTYPECOL         ARB_NW_BE ARB_NW_BE,
        ADDRNUMBERCOL     ARB_NW_BE ARB_NW_BE,
* AT
        AVKCODE           ARB_NW_AT ARB_NW_AT,
        SPEZIFIKATION     ARB_NW_AT ARB_NW_AT,
* AU
        PACKAGINGTYPE     ARB_NW_AU ARB_NW_AU,
        DANGEROUSGOODS    ARB_NW_AU ARB_NW_AU,
        WASTECLASS        ARB_NW_AU ARB_NW_AU,
        TRANSPORTTYPE     ARB_NW_AU ARB_NW_AU,
        PACKAGENUMBER     ARB_NW_AU ARB_NW_AU.

    concatenate
      'PARTNER_ER=PARTNER_ERZ,IDENT_ER=IDENT_ERZ,'
      'PARTNER_BF=PARTNER_BEF,IDENT_BF=IDENT_BEF,'
      'PARTNER_EN=PARTNER_ENT,IDENT_EN=IDENT_ENT,'
      'LFDNR=AVVLFDNR'
    into FNWALIASES.
  endif.
endmethod.


method SELECT_DATA_SPEC.
  data:
    LREADER type /WATP/SOBJREAD_KEY,
    LFIELDS type STRING,
    LKEYS   type standard table of /WATP/SARBNW_KEY.

    SECURITY_DATA.

  clear PAR_LIST.

  if FRESULT_IS_EMPTY is not initial.
    exit. " leave, because ranges define a empty result set
  endif.

  if FREADER->OBJTYPE = 'ARB_NW'. " only search for a special reader if the NW reader base is used
    LREADER = GET_RESPONSIBLE_NW_READER( ).
    if LREADER-OBJTYPE <> FREADER->OBJTYPE. " is another reader responsible?
      if LREADER is initial. " no reader for the range combination available
        return. " return empty set
      endif.
      /WATP/CL_OBJECTREADER=>CL_READ( " use a special NW reader
        exporting
          PAR_OBJTYPE   = LREADER-OBJTYPE
          PAR_OBJREADER = LREADER-OBJREADER
          PAR_RANGES    = FRANGES
          PAR_BOCACHE   = FBOCACHE
          PAR_MAXROWS   = FMAXROWS
          PAR_AUTHORITYMODE = FAUTHORITYMODE
        importing
          PAR_TABLE     = PAR_LIST
      ).
      return. " we are done
    endif.
  endif.

* default read algorithm for NW
  READ_BO_RANGE FRANGES:
    STATUS, " Status
    NWNR,
    NWINTNR, " PKEY
    OBJNR, " Objektnummer
    UUID_DOC,
    PROOFTYPE,
    NWVALIDTO,
    NWVALIDFROM,
    AVVCODE,
    DEFMATNR,
    ADRUNCANR,
    PARTNER_ERZ,
    PARTNER_BEF,
    PARTNER_ENT,
    DEVLOC_ANS,
    WDPLANT,
    LOEVM. " Löschvermerk

  READ_BO_RANGE_KEYS FRANGES __KEYS LKEYS.

  clear PAR_LIST.

  if <LSTATUSRANGE> is not initial or
     EXISTS_FIELD( 'STATUS' ) is not initial or
     EXISTS_FIELD( 'OBJTYPE' ) is not initial.
    LFIELDS = /WATP/CL_SQL_MISC=>GETSELECTFIELDLIST(
      PAR_WORKAREA = PAR_LIST
      PAR_FIELDALIAS = FNWALIASES
      PAR_QUERYTABLES = '/WATP/TARBNW as NW,/WATP/VOBJSTATN as OS'
    ).
    if LKEYS is not initial and lines( LKEYS ) > 0.
      "SECURITY: LFIELDS is a list of the field intersection of the structures of PAR_LIST and /WATP/TARBNW, /WATP/VOBJSTATN (see above) => DDIC-based and not injection possible
      "SECURITY: field-value check
       SECURITY_FIELDCHECK LFIELDS.

      select (LFIELDS)
      into corresponding fields of table PAR_LIST
      from /WATP/TARBNW as NW
      inner join /WATP/VOBJSTATN as OS
         on NW~OBJNR = OS~OBJNR
      up to FMAXROWS rows
      for all entries in LKEYS
      where NW~NWINTNR     =  LKEYS-NWINTNR
        and NW~NWNR        in <LNWNRRANGE>
        and NW~NWINTNR     in <LNWINTNRRANGE>
        and NW~UUID_DOC    in <LUUID_DOCRANGE>
        and NW~PROOFTYPE   in <LPROOFTYPERANGE>
        and NW~NWVALIDTO   in <LNWVALIDTORANGE>
        and NW~NWVALIDFROM in <LNWVALIDFROMRANGE>
        and NW~AVVCODE     in <LAVVCODERANGE>
        and NW~DEFMATNR    in <LDEFMATNRRANGE>
        and NW~ADRUNCANR   in <LADRUNCANRRANGE>
        and NW~PARTNER_ERZ in <LPARTNER_ERZRANGE>
        and NW~PARTNER_BEF in <LPARTNER_BEFRANGE>
        and NW~PARTNER_ENT in <LPARTNER_ENTRANGE>
        and NW~DEVLOC_ANS  in <LDEVLOC_ANSRANGE>
        and NW~WDPLANT     in <LWDPLANTRANGE>
        and NW~OBJNR       in <LOBJNRRANGE>
        and OS~STATUS      in <LSTATUSRANGE>
        and NW~LOEVM       in <LLOEVMRANGE>.
    else.
      "SECURITY: LFIELDS is a list of the field intersection of the structures of PAR_LIST and /WATP/TARBNW, /WATP/VOBJSTATN (see above) => DDIC-based and not injection possible
      "SECURITY: field-value check
      SECURITY_FIELDCHECK LFIELDS.

      select (LFIELDS)
      into corresponding fields of table PAR_LIST
      from /WATP/TARBNW as NW
      inner join /WATP/VOBJSTATN as OS
         on NW~OBJNR = OS~OBJNR
      up to FMAXROWS rows
      where NW~NWNR        in <LNWNRRANGE>
        and NW~NWINTNR     in <LNWINTNRRANGE>
        and NW~UUID_DOC    in <LUUID_DOCRANGE>
        and NW~PROOFTYPE   in <LPROOFTYPERANGE>
        and NW~NWVALIDTO   in <LNWVALIDTORANGE>
        and NW~NWVALIDFROM in <LNWVALIDFROMRANGE>
        and NW~AVVCODE     in <LAVVCODERANGE>
        and NW~DEFMATNR    in <LDEFMATNRRANGE>
        and NW~ADRUNCANR   in <LADRUNCANRRANGE>
        and NW~PARTNER_ERZ in <LPARTNER_ERZRANGE>
        and NW~PARTNER_BEF in <LPARTNER_BEFRANGE>
        and NW~PARTNER_ENT in <LPARTNER_ENTRANGE>
        and NW~DEVLOC_ANS  in <LDEVLOC_ANSRANGE>
        and NW~WDPLANT     in <LWDPLANTRANGE>
        and NW~OBJNR       in <LOBJNRRANGE>
        and OS~STATUS      in <LSTATUSRANGE>
        and NW~LOEVM       in <LLOEVMRANGE>.
    endif.
  else.
    LFIELDS = /WATP/CL_SQL_MISC=>GETSELECTFIELDLIST(
      PAR_WORKAREA = PAR_LIST
      PAR_FIELDALIAS = FNWALIASES
      PAR_QUERYTABLES = '/WATP/TARBNW as NW'
    ).
    if LKEYS is not initial and lines( LKEYS ) > 0.
      "SECURITY: LFIELDS is a list of the field intersection of the structures of PAR_LIST and /WATP/TARBNW (see above) => DDIC-based and not injection possible
      "SECURITY: field-value check
      SECURITY_FIELDCHECK LFIELDS.
      select (LFIELDS)
      into corresponding fields of table PAR_LIST
      from /WATP/TARBNW as NW
      up to FMAXROWS rows
      for all entries in LKEYS
      where NW~NWINTNR     =  LKEYS-NWINTNR
        and NW~NWNR        in <LNWNRRANGE>
        and NW~NWINTNR     in <LNWINTNRRANGE>
        and NW~UUID_DOC    in <LUUID_DOCRANGE>
        and NW~PROOFTYPE   in <LPROOFTYPERANGE>
        and NW~NWVALIDTO   in <LNWVALIDTORANGE>
        and NW~NWVALIDFROM in <LNWVALIDFROMRANGE>
        and NW~AVVCODE     in <LAVVCODERANGE>
        and NW~DEFMATNR    in <LDEFMATNRRANGE>
        and NW~ADRUNCANR   in <LADRUNCANRRANGE>
        and NW~PARTNER_ERZ in <LPARTNER_ERZRANGE>
        and NW~PARTNER_BEF in <LPARTNER_BEFRANGE>
        and NW~PARTNER_ENT in <LPARTNER_ENTRANGE>
        and NW~DEVLOC_ANS  in <LDEVLOC_ANSRANGE>
        and NW~WDPLANT     in <LWDPLANTRANGE>
        and NW~OBJNR       in <LOBJNRRANGE>
        and NW~LOEVM       in <LLOEVMRANGE>.
    else.
      select *"(LFIELDS)
      into corresponding fields of table PAR_LIST
      from /WATP/TARBNW as NW
      up to FMAXROWS rows
      where NW~NWNR        in <LNWNRRANGE>
        and NW~NWINTNR     in <LNWINTNRRANGE>
        and NW~UUID_DOC    in <LUUID_DOCRANGE>
        and NW~PROOFTYPE   in <LPROOFTYPERANGE>
        and NW~NWVALIDTO   in <LNWVALIDTORANGE>
        and NW~NWVALIDFROM in <LNWVALIDFROMRANGE>
        and NW~AVVCODE     in <LAVVCODERANGE>
        and NW~DEFMATNR    in <LDEFMATNRRANGE>
        and NW~ADRUNCANR   in <LADRUNCANRRANGE>
        and NW~PARTNER_ERZ in <LPARTNER_ERZRANGE>
        and NW~PARTNER_BEF in <LPARTNER_BEFRANGE>
        and NW~PARTNER_ENT in <LPARTNER_ENTRANGE>
        and NW~DEVLOC_ANS  in <LDEVLOC_ANSRANGE>
        and NW~WDPLANT     in <LWDPLANTRANGE>
        and NW~OBJNR       in <LOBJNRRANGE>
        and NW~LOEVM       in <LLOEVMRANGE>.
    endif.
  endif.

endmethod.
ENDCLASS.


<img width="1763" height="889" alt="image" src="https://github.com/user-attachments/assets/5ca44bbf-7fb4-4a62-ba63-73b6951014be" />

<img width="1755" height="868" alt="image" src="https://github.com/user-attachments/assets/a235fd12-bc19-4af2-b869-9ced6269d920" />

<img width="1722" height="751" alt="image" src="https://github.com/user-attachments/assets/ad83a9eb-a5a5-46b7-bbde-04574014f009" />







<img width="1736" height="827" alt="image" src="https://github.com/user-attachments/assets/56392580-f878-4e17-8626-b80a9d417ba9" />


What I'd do first
