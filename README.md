class ZCL_WR_TA_ORDER_GROUPING definition
  public
  inheriting from /WATP/CL_TRANSACTION
  create public .

public section.

*  types:
*    TT_POBJNR type standard table of EWA_ORDER_OBJECT-POBJNR with default key .
  constants CTA_ORDER_GROUPING type /WATP/DOBJTRANSACTION value 'ZWR_TA_ORDER_GROUP' ##NO_TEXT.
*  data ORDERPOS type TT_POBJNR .
  data ORDERS type /WATP/PEWAORDER_KEYS .

  class-methods CL_BOOK_GROUP_ORDER
    importing
      !PAR_ORDERS type /WATP/PEWAORDER_KEYS
    raising
      /WATP/CX_BASE .
  class-methods CL_NORMALIZE_ROUTESEQ
    importing
      !IV_ROUTE_SEQUENCE type ROUTE_SEQUENCE
    returning
      value(RV_RESULT) type ROUTE_SEQUENCE .
  class-methods CL_ROUTESEQ_FROM_INT
    importing
      !IV_AS_INT type I
    returning
      value(RV_RESULT) type ROUTE_SEQUENCE .
  class-methods CL_SORT_BY_SEQUENCE
    importing
      value(PAR_TABLE) type STANDARD TABLE
    exporting
      !PAR_RESULT type STANDARD TABLE .
protected section.

  types:
    begin of TS_GROUP_DATA,
      ZZ_TECH_FACHBE type EWA_ORDER_OBJECT-ZZ_TECH_FACHBE,
      KUNNR          type EWA_ORDER_OBJECT-KUNNR,
      SERVLOC        type EWA_ORDER_OBJECT-SERVLOC,
      TIMEFRAME      type EWA_ORDER_OBJECT-TIMEFRAME,
      START_TIME     type EWA_ORDER_OBJECT-START_TIME,
      END_TIME       type EWA_ORDER_OBJECT-END_TIME,
    end of TS_GROUP_DATA .
  types:
    begin of TS_GROUP_MEMBER,
      POBJNR   type EWA_ORDER_OBJECT-POBJNR,
      IS_MAIN  type ABAP_BOOL,
      SEQUENCE type INT2,
      DATA_REF type ref to DATA,
    end of TS_GROUP_MEMBER .
  types:
    TT_GROUP_MEMBER type hashed table of TS_GROUP_MEMBER with unique key POBJNR
                                                         with non-unique sorted key SEQUENCE components SEQUENCE .
  types:
    begin of TS_GROUP,
      HASH     type STRING,
      MEMBERS  type TT_GROUP_MEMBER,
      SEQUENCE type I,
      DATA_REF type ref to DATA,
    end of TS_GROUP .
  types:
    TT_GROUP type hashed table of TS_GROUP with unique key HASH .
  types:
    begin of TPOSUPDFIELDS.
      include type ZWR_S_EORD_WDOI_GROUPING_DATA as ZGROUPINGDATA.
  types:
    end of TPOSUPDFIELDS .
  types:
    begin of TS_POSDATA,
      POBJNR               type J_OBJNR,
      ORDERNR              type EWA_ORDER_OBJECT-ORDERNR,
      ORDER_LAUFNR         type EWA_ORDER_OBJECT-ORDER_LAUFNR,
      ZZ_TECH_FACHBE       type EWA_ORDER_OBJECT-ZZ_TECH_FACHBE,
      SERVFREQNR           type EWA_ORDER_OBJECT-SERVFREQNR,
      SERVFREQ_LFNR        type EWA_ORDER_OBJECT-SERVFREQ_LFNR,
      KUNNR                type EWA_ORDER_OBJECT-KUNNR,
      SERVLOC              type EWA_ORDER_OBJECT-SERVLOC,
      TIMEFRAME            type EWA_ORDER_OBJECT-TIMEFRAME,
      START_TIME           type EWA_ORDER_OBJECT-START_TIME,
      END_TIME             type EWA_ORDER_OBJECT-END_TIME,
      ROUTE_SEQUENCE       type EWA_ORDER_OBJECT-ROUTE_SEQUENCE,
      SDAUFNR              type EWA_ORDER_OBJECT-SDAUFNR,
      SDPOSNR              type EWA_ORDER_OBJECT-SDPOSNR.
      include              type TPOSUPDFIELDS as UPDATEFIELDS.
  types:
      ZZ_POBJNR_MAIN_OLD   type EWA_ORDER_OBJECT-ZZ_POBJNR_MAIN,
      ZZ_ROUTESEQ_MAIN_OLD type EWA_ORDER_OBJECT-ZZ_ROUTESEQ_MAIN,
      ZZ_POBJNR_COMM_OLD   type EWA_ORDER_OBJECT-ZZ_POBJNR_COMM,
      HAS_NEW_SEQ          type KENNZX,
      GENERIC_FREQUENCY    type EWAOBJH_GENERIC_FREQUENCY.
  types:
    end of TS_POSDATA .
  types:
    TT_POSDATA type standard table of TS_POSDATA with key POBJNR .
  types:
    begin of TS_POSFREQ,
      POBJNR type J_OBJNR.
      include type EWAOBJH_GENERIC_FREQUENCY as GENERIC_FREQUENCY.
  types:
    end of TS_POSFREQ .
  types:
    TT_POSFREQ type standard table of TS_POSFREQ with key POBJNR .

*    TPOSDATAS type sorted table of TPOSDATA
*      with unique key POBJNR
*      with non-unique sorted key SGROUPING components KUNNR SERVLOC TIMEFRAME START_TIME END_TIME ORDER_LAUFNR.
  methods ACQUIRE_POSDATAS
    importing
      !PAR_BO          type ref to /WATP/CL_BO_ORDER
    changing
      value(PAR_TABLE) type TT_POSDATA
    raising
      /WATP/CX_BASE .
  methods BOOK_GROUP_ORDERS
    raising
      /WATP/CX_BASE .
  methods CHECK_GROUP_ORDERS
    raising
      /WATP/CX_BASE .
  methods DO_GROUP_ORDERS
    raising
      /WATP/CX_BASE .
  methods FILL_GROUPING_FIELDS
    changing
      !PAR_TABLE type TT_POSDATA
    raising
      /WATP/CX_BASE .
  methods FILTER_POSDATAS
    changing
      !PAR_TABLE type TT_POSDATA
    raising
      /WATP/CX_BASE .
  methods GROUP_ORDER
    importing
      !PAR_ORDER type /WATP/SEWAORDER_KEY
    raising
      /WATP/CX_BASE .
  methods IS_GROUPING_ALLOWED
    importing
      !IR_POSDATA      type ref to TS_POSDATA
    returning
      value(RV_RESULT) type ABAP_BOOL .
  methods UPDATE_POSITIONS
    importing
      !PAR_BO    type ref to /WATP/CL_BO_ORDER
    changing
      !PAR_TABLE type TT_POSDATA
    raising
      /WATP/CX_BASE .

  methods CALL_METHOD
      redefinition .
  methods GETASSIGN
      redefinition .
private section.
ENDCLASS.



CLASS ZCL_WR_TA_ORDER_GROUPING IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->ACQUIRE_POSDATAS
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_BO                         TYPE REF TO /WATP/CL_BO_ORDER
* | [<-->] PAR_TABLE                      TYPE        TT_POSDATA
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method ACQUIRE_POSDATAS.
  field-symbols:
    <TABLE> type any table.

  clear PAR_TABLE.
  data(LR_TABLEREF) = PAR_BO->GETDETAILTABLE( /WATP/CL_BO_ORDER=>CPOSITIONS ).
  if LR_TABLEREF is not bound.
    return.
  endif.
  assign LR_TABLEREF->* to <TABLE>.
  if SY-SUBRC is not initial or LINES( <TABLE> ) = 0.
    return.
  endif.
  PAR_TABLE = corresponding #( <TABLE> ).
  check LINES( PAR_TABLE ) > 0.
  data(LT_POSFREQ) = value TT_POSFREQ( ).
  select *
    from EWA_ORDER_OBJECT as OI
   inner join EWAOBJ as SFH
      on SFH~OBJNR = OI~SERVFREQNR
   inner join EWAOBJH as SFI
      on SFI~OBJNR  = OI~SERVFREQNR
     and SFI~LAUFNR = OI~SERVFREQ_LFNR
     and ( SFI~AB <= OI~OLD_ORDER_DATE and SFI~BIS >= OI~OLD_ORDER_DATE )
     for all entries in @PAR_TABLE
   where OI~POBJNR = @PAR_TABLE-POBJNR
    into corresponding fields of table @LT_POSFREQ.
  loop at PAR_TABLE assigning field-symbol(<DATA>).
    <DATA>-ZZ_POBJNR_MAIN_OLD = <DATA>-ZZ_POBJNR_MAIN.
    <DATA>-ZZ_ROUTESEQ_MAIN_OLD = <DATA>-ZZ_ROUTESEQ_MAIN.
    <DATA>-ZZ_POBJNR_COMM_OLD = <DATA>-ZZ_POBJNR_COMM.
    try.
        data(LS_POSFREQ) = LT_POSFREQ[ POBJNR = <DATA>-POBJNR ].
        <DATA>-GENERIC_FREQUENCY = corresponding #( LS_POSFREQ-GENERIC_FREQUENCY ).
      catch CX_SY_ITAB_LINE_NOT_FOUND ##NO_HANDLER.
    endtry.
  endloop.
endmethod.                                               "#EC CI_VALPAR


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->BOOK_GROUP_ORDERS
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method BOOK_GROUP_ORDERS.
  CHECK_GROUP_ORDERS( ).
  BEGIN_UPDATE( ).
  try.
      DO_GROUP_ORDERS( ).
    cleanup.
      END_UPDATE( PAR_FAILED = ABAP_TRUE ).
  endtry.
  END_UPDATE( PAR_FAILED = ABAP_FALSE ).
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->CALL_METHOD
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NAME                       TYPE        CSEQUENCE
* | [--->] PAR_PARAMS                     TYPE        ABAP_PARMBIND_TAB(optional)
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CALL_METHOD.
  TA_CALL_METHOD.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->CHECK_GROUP_ORDERS
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CHECK_GROUP_ORDERS.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_TA_ORDER_GROUPING=>CL_BOOK_GROUP_ORDER
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_ORDERS                     TYPE        /WATP/PEWAORDER_KEYS
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_BOOK_GROUP_ORDER.
  TA_BEGIN_NAMED_VAR TA ZCL_WR_TA_ORDER_GROUPING=>CTA_ORDER_GROUPING.
    TA_NO_COMMIT TA.
    TA_DO_RAISE_EXCEPTION TA.
    TA_SET_PARAM TA ORDERS PAR_ORDERS.
    TA_EXECUTE TA BOOK_GROUP_ORDERS.
  TA_END TA.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_TA_ORDER_GROUPING=>CL_NORMALIZE_ROUTESEQ
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_ROUTE_SEQUENCE              TYPE        ROUTE_SEQUENCE
* | [<-()] RV_RESULT                      TYPE        ROUTE_SEQUENCE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_NORMALIZE_ROUTESEQ.
  data LV_ROUTE_SEQUENCE type ROUTE_SEQUENCE.
  LV_ROUTE_SEQUENCE = IV_ROUTE_SEQUENCE.
  LV_ROUTE_SEQUENCE = CONDENSE( LV_ROUTE_SEQUENCE ).
  RV_RESULT = |{ LV_ROUTE_SEQUENCE width = 4 align = right pad = '0' }| ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_TA_ORDER_GROUPING=>CL_ROUTESEQ_FROM_INT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_AS_INT                      TYPE        I
* | [<-()] RV_RESULT                      TYPE        ROUTE_SEQUENCE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_ROUTESEQ_FROM_INT.
  RV_RESULT = |{ IV_AS_INT width = 4 align = right pad   = '0' }| ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_TA_ORDER_GROUPING=>CL_SORT_BY_SEQUENCE
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_TABLE                      TYPE        STANDARD TABLE
* | [<---] PAR_RESULT                     TYPE        STANDARD TABLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_SORT_BY_SEQUENCE.
  types: begin of TS_SORTING_KEY,
           ZZ_ROUTESEQ_MAIN type EWA_ORDER_OBJECT-ZZ_ROUTESEQ_MAIN,
           ROUTE_SEQUENCE   type EWA_ORDER_OBJECT-ROUTE_SEQUENCE,
           SORT_SEQUENCE    type EWA_ORDER_OBJECT-ZZ_ROUTESEQ_MAIN,
           LINE             type DATAREF.
           include type EWA_ORDER_OBJECT_IKEY as IKEY.
  types: end of TS_SORTING_KEY.
  types: TT_SORTING_KEY type standard table of TS_SORTING_KEY with empty key.
  data(LT_SORTING_KEY) = value TT_SORTING_KEY( ).
  data(LR_LINE) = value DATAREF( ).
  loop at PAR_TABLE reference into LR_LINE.
    assign LR_LINE->* to field-symbol(<LS_LINE>).
    assert SY-SUBRC = 0.
    append value #( base corresponding #( <LS_LINE> except SORT_SEQUENCE
                                                           LINE )
                    LINE = LR_LINE )
           to LT_SORTING_KEY.
  endloop.
  loop at LT_SORTING_KEY assigning field-symbol(<LS_LINE_FOR_ORDERNR>)
       group by <LS_LINE_FOR_ORDERNR>-ORDERNR into data(LV_ORDERNR).
    data(LV_HAVE_COL_SEQUENCE) = ABAP_FALSE.
    loop at group LV_ORDERNR assigning field-symbol(<LS_SORTING_KEY>)
         where ZZ_ROUTESEQ_MAIN is not initial.
      LV_HAVE_COL_SEQUENCE = ABAP_TRUE.
      exit.
    endloop.
    if LV_HAVE_COL_SEQUENCE = ABAP_TRUE.
      loop at group LV_ORDERNR assigning <LS_SORTING_KEY>.
        <LS_SORTING_KEY>-SORT_SEQUENCE = <LS_SORTING_KEY>-ZZ_ROUTESEQ_MAIN.
      endloop.
    else.
      loop at group LV_ORDERNR assigning <LS_SORTING_KEY>.
        <LS_SORTING_KEY>-SORT_SEQUENCE = <LS_SORTING_KEY>-ROUTE_SEQUENCE.
      endloop.
    endif.
  endloop.
  sort LT_SORTING_KEY stable by ORDERNR SORT_SEQUENCE.
  clear PAR_RESULT.
  loop at LT_SORTING_KEY assigning <LS_SORTING_KEY>.
    assign <LS_SORTING_KEY>-LINE->* to <LS_LINE>.
    assert SY-SUBRC = 0.
    append <LS_LINE> to PAR_RESULT.
  endloop.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->DO_GROUP_ORDERS
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method DO_GROUP_ORDERS.
  loop at ORDERS assigning field-symbol(<ORDER>).
    GROUP_ORDER( <ORDER> ).
  endloop.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->FILL_GROUPING_FIELDS
* +-------------------------------------------------------------------------------------------------+
* | [<-->] PAR_TABLE                      TYPE        TT_POSDATA
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method FILL_GROUPING_FIELDS.

  data LR_GROUP type ref to TS_GROUP.
  data LR_MEMBER type ref to TS_GROUP_MEMBER.

  field-symbols:
    <OI>   type TS_POSDATA,
    <DATA> type DATA.

  if LINES( PAR_TABLE ) = 0.
    return.
  endif.

  data(LT_GROUPS) = value TT_GROUP( ).
  sort PAR_TABLE by ZZ_TECH_FACHBE KUNNR SERVLOC TIMEFRAME START_TIME END_TIME ORDER_LAUFNR.
  loop at PAR_TABLE assigning <OI>.
    clear: LR_GROUP, LR_MEMBER.
    unassign <DATA>.
    data(LS_GROUP_DATA) = corresponding TS_GROUP_DATA( <OI> ).
    data(LV_HASH) = ZCL_WR_HASH=>CL_GET_HASH_FOR_DATA( PAR_DATA = LS_GROUP_DATA ).
    try.
        LR_GROUP = ref #( LT_GROUPS[ HASH = LV_HASH ] ).
      catch CX_SY_ITAB_LINE_NOT_FOUND.
        insert value #( HASH = LV_HASH SEQUENCE = ( LINES( LT_GROUPS ) + 1 ) ) into table LT_GROUPS reference into LR_GROUP.
        create data LR_GROUP->DATA_REF type TS_GROUP_DATA.
        assign LR_GROUP->DATA_REF->* to <DATA>.
        <DATA> = LS_GROUP_DATA.
    endtry.
    insert value #( POBJNR = <OI>-POBJNR SEQUENCE = ( LINES( LR_GROUP->MEMBERS ) + 1 ) ) into table LR_GROUP->MEMBERS reference into LR_MEMBER.
    if LINES( LR_GROUP->MEMBERS ) = 1.
      LR_MEMBER->IS_MAIN = ABAP_TRUE.
    endif.
    LR_MEMBER->DATA_REF = ref #( <OI> ).
  endloop.
  loop at LT_GROUPS assigning field-symbol(<GROUP>).
    loop at <GROUP>-MEMBERS assigning field-symbol(<MEMBER_MAIN>) using key SEQUENCE.
      loop at <GROUP>-MEMBERS assigning field-symbol(<MEMBER>) using key SEQUENCE.
        unassign <OI>.
        assign <MEMBER>-DATA_REF->* to <OI>.
        <OI>-ZZ_POBJNR_MAIN = <MEMBER_MAIN>-POBJNR.
      endloop.
      exit.
    endloop.
  endloop.

  data:
    LCOL_INDEX      type I,
    LCURRENT_POBJNR type J_OBJNR,
    LR_POS          type ref to TS_POSDATA.

  LCOL_INDEX = 0.
  "1. Aus COLs entfernte Positionen an den Anfang stellen
  loop at PAR_TABLE reference into LR_POS where ZZ_POBJNR_MAIN_OLD is not initial.
    if LR_POS->ZZ_POBJNR_MAIN = LR_POS->POBJNR and LR_POS->ZZ_POBJNR_MAIN_OLD <> LR_POS->ZZ_POBJNR_MAIN.
      LCOL_INDEX = LCOL_INDEX + 1.
      LR_POS->ZZ_ROUTESEQ_MAIN = CL_ROUTESEQ_FROM_INT( LCOL_INDEX ).
      LR_POS->HAS_NEW_SEQ = ABAP_TRUE.
    endif.
  endloop.
  "2. Neue (Haupt-)Position ohne ROUTE_SEQUENCE auch an den Anfang
  loop at PAR_TABLE reference into LR_POS where ZZ_ROUTESEQ_MAIN_OLD is initial and ROUTE_SEQUENCE is initial and HAS_NEW_SEQ is initial.
    if LR_POS->ZZ_POBJNR_MAIN = LR_POS->POBJNR.
      LCOL_INDEX = LCOL_INDEX + 1.
      LR_POS->ZZ_ROUTESEQ_MAIN = CL_ROUTESEQ_FROM_INT( LCOL_INDEX ).
      LR_POS->HAS_NEW_SEQ = ABAP_TRUE.
    endif.
  endloop.
  sort PAR_TABLE by ZZ_ROUTESEQ_MAIN_OLD ZZ_POBJNR_MAIN ORDER_LAUFNR.
  "3. (Haupt-)Position ohne ROUTE_SEQUENCE nach bisheriger COL-Sequenz einsortieren
  loop at PAR_TABLE reference into LR_POS where ZZ_ROUTESEQ_MAIN_OLD is not initial and ROUTE_SEQUENCE is initial and HAS_NEW_SEQ is initial.
    if LR_POS->ZZ_POBJNR_MAIN = LR_POS->POBJNR.
      LCOL_INDEX = LCOL_INDEX + 1.
      LR_POS->ZZ_ROUTESEQ_MAIN = CL_ROUTESEQ_FROM_INT( LCOL_INDEX ).
      LR_POS->HAS_NEW_SEQ = ABAP_TRUE.
    endif.
  endloop.
  "4. Für alle Sub-Positionen die Sequenz aus Kopf übernehmen (für schon sequenzierte Hauptpositionen)
  loop at PAR_TABLE reference into LR_POS.
    if LR_POS->ZZ_POBJNR_MAIN <> LR_POS->POBJNR.
      read table PAR_TABLE reference into data(LR_MAIN_POS) with key POBJNR = LR_POS->ZZ_POBJNR_MAIN.
      if SY-SUBRC is initial and LR_MAIN_POS->HAS_NEW_SEQ is not initial.
        LR_POS->ZZ_ROUTESEQ_MAIN = LR_MAIN_POS->ZZ_ROUTESEQ_MAIN.
        LR_POS->HAS_NEW_SEQ = ABAP_TRUE.
      endif.
    endif.
  endloop.
  sort PAR_TABLE by ZZ_ROUTESEQ_MAIN ZZ_POBJNR_MAIN ORDER_LAUFNR.
  "5. jetzt für den Rest die Sequenz anpassen:
  loop at PAR_TABLE reference into LR_POS where HAS_NEW_SEQ is initial.
    if LCURRENT_POBJNR is initial or LCURRENT_POBJNR <> LR_POS->ZZ_POBJNR_MAIN.
      LCURRENT_POBJNR = LR_POS->ZZ_POBJNR_MAIN.
      LCOL_INDEX = LCOL_INDEX + 1.
    endif.
    LR_POS->ZZ_ROUTESEQ_MAIN = CL_ROUTESEQ_FROM_INT( LCOL_INDEX ).
  endloop.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->FILTER_POSDATAS
* +-------------------------------------------------------------------------------------------------+
* | [<-->] PAR_TABLE                      TYPE        TT_POSDATA
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method FILTER_POSDATAS.
  loop at PAR_TABLE assigning field-symbol(<OI>).
    if IS_GROUPING_ALLOWED( IR_POSDATA = ref #( <OI> ) ).
      continue.
    endif.
    delete PAR_TABLE.
  endloop.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->GETASSIGN
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_PATH                       TYPE        CSEQUENCE
* | [<-()] PAR_ANY                        TYPE REF TO DATA
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GETASSIGN .
  TA_GETASSIGN.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->GROUP_ORDER
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_ORDER                      TYPE        /WATP/SEWAORDER_KEY
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GROUP_ORDER.
  data:
    LBO         type ref to /WATP/CL_BO_ORDER,
    LT_POSDATAS type TT_POSDATA.

  LBO ?= GETBO( PAR_OBJTYPE = /WATP/CL_BO_ORDER=>COT_TPORDER
                PAR_KEY     = PAR_ORDER
                PAR_DETAILS = /WATP/CL_BO_ORDER=>CPOSITIONS ).
  try.
      ACQUIRE_POSDATAS(
        exporting
          PAR_BO    = LBO
        changing
          PAR_TABLE = LT_POSDATAS
      ).
      FILTER_POSDATAS(
        changing
          PAR_TABLE = LT_POSDATAS
      ).
      FILL_GROUPING_FIELDS(
        changing
          PAR_TABLE = LT_POSDATAS
      ).
      CL_SORT_BY_SEQUENCE( exporting PAR_TABLE  = LT_POSDATAS
                           importing PAR_RESULT = LT_POSDATAS ).
      UPDATE_POSITIONS(
        exporting
          PAR_BO    = LBO
        changing
          PAR_TABLE = LT_POSDATAS
      ).
    cleanup.
      UNLOCK_BO( LBO ).
  endtry.
  UNLOCK_BO( LBO ).
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->IS_GROUPING_ALLOWED
* +-------------------------------------------------------------------------------------------------+
* | [--->] IR_POSDATA                     TYPE REF TO TS_POSDATA
* | [<-()] RV_RESULT                      TYPE        ABAP_BOOL
* +--------------------------------------------------------------------------------------</SIGNATURE>
method IS_GROUPING_ALLOWED.
  RV_RESULT = ABAP_FALSE.
  if IR_POSDATA->ZZ_TECH_FACHBE is not initial.
    try.
        data(LS_TECH_FB) = ZCL_WR_EEWA_EWMD_MISC=>GT_TECH_FB[ TECH_FACHBE = IR_POSDATA->ZZ_TECH_FACHBE ].
      catch CX_SY_ITAB_LINE_NOT_FOUND.
        return.
    endtry.
    RV_RESULT = BOOLC( LS_TECH_FB-GROUP_ORDERS is not initial
                       and IR_POSDATA->GENERIC_FREQUENCY-DAY_FLAG is not initial
                       and IR_POSDATA->GENERIC_FREQUENCY-DAY_TYPE = '1' ) ##NO_TEXT.
    return.
  endif.
  RV_RESULT = ABAP_FALSE.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_TA_ORDER_GROUPING->UPDATE_POSITIONS
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_BO                         TYPE REF TO /WATP/CL_BO_ORDER
* | [<-->] PAR_TABLE                      TYPE        TT_POSDATA
* | [!CX!] /WATP/CX_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method UPDATE_POSITIONS.
  check LINES( PAR_TABLE ) > 0.
  data(LDETAILORDER) = value /WATP/PINTS( for <LGROUPINGINFO> in PAR_TABLE ( -1 ) ).
  do PAR_BO->GETDETAILRECORDCOUNT( 'POSITIONS' ) times.
    data(LINDEX) = SY-INDEX.
    PAR_BO->POSITIONS_CHANGING( exporting PAR_INDEX   = LINDEX
                                importing PAR_DATAREF = data(LDETAILRECORD_UNTYPED) ).
    data(LDETAILRECORD) = cast /WATP/SDTL_EWAORDERPOS( LDETAILRECORD_UNTYPED ).
    read table PAR_TABLE reference into data(LGROUPINGINFO)
         with table key POBJNR = LDETAILRECORD->POBJNR.
    if SY-SUBRC <> 0.
      "ungruppierte Positionen ans Ende?
      append LINDEX to LDETAILORDER.
    else.
      LDETAILORDER[ SY-TABIX ] = LINDEX.
      /WATP/CL_MISC=>MOVE_DATA( exporting PAR_FROMDATA   = LGROUPINGINFO->UPDATEFIELDS
                                          PAR_TOREF      = LDETAILRECORD
                                          PAR_FIELDSFROM = 'S'
                                importing PAR_CHANGEDDATAFIELDS = data(LT_CHANGEDDATAFIELDS) ).
      if LINES( LT_CHANGEDDATAFIELDS ) > 0.
        PAR_BO->POSITIONS_CHANGED( PAR_INDEX = LINDEX ).
      endif.
    endif.
  enddo.
  "Damit Verschiebeoperationen korrekt funktionieren,
  "muss die Standard-Sequenzierung zur COL-Sequenzierung passen
  if LINES( LDETAILORDER ) > 0.
    PAR_BO->BOOK_SET_SEQUENCE( PAR_INDICES = LDETAILORDER ).
  endif.
  PAR_BO->SAVE_DATA( ).
endmethod.
ENDCLASS.
