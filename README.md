Three things, in this order.

1. One last lookup (5 min) — SE24 → /WATP/CL_BO_HW_WDPLANT → method CHECK_AVV_DELIVERY_STATIC.
Read which table it selects from. Then SE16 that table.

Has entries, one facility per EWC code → the 67 is deliverable after all; come back to me with the table name.
Empty → WDF stays manual. Go to step 3 and say so.


class /WATP/CL_BO_HW_WDPLANT definition
  public
  inheriting from /WATP/CL_BO_WDPLANT
  create public .

*"* public components of class /WATP/CL_BO_HW_WDPLANT
*"* do not include other source files here!!!
public section.

  constants CDETAIL_AVKS type CHAR30 value 'AVKS' ##NO_TEXT.
  constants CDETAIL_IDENTS type CHAR30 value 'IDENTS' ##NO_TEXT.
  constants CDETAIL_AVVS type CHAR30 value 'AVVS' ##NO_TEXT.
  data DATAREF_ADRTP type ref to /WATP/SADDRESS .
  constants C_RELATION_ADRTP type CHAR30 value 'ADRTP' ##NO_TEXT.

  class-methods CHECK_AVV_DELIVERY_STATIC
    importing
      !PAR_WDPLANTNR type EWAEL_WDPLANTNR
      !PAR_AVVCODE type /WATP/DAVVCODE
    raising
      /WATP/CX_BASE .
  methods INSERT_PARENT_IDENTS .
  methods IDENTS_CHANGED
    importing
      !PAR_INDEX type INT4
    raising
      /WATP/CX_BASE .
  methods VALIDATE_RELATION_ADRTP .
  methods AVKS_CHANGED
    importing
      !PAR_INDEX type INT4
    raising
      /WATP/CX_BASE .
  methods AVVS_CHANGED
    importing
      !PAR_INDEX type INT4
    raising
      /WATP/CX_BASE .
  methods IDENTS_CHANGING
    importing
      !PAR_INDEX type INT4
    exporting
      !PAR_ITEM type ref to DATA
    raising
      /WATP/CX_BASE .
  methods AVKS_CHANGING
    importing
      !PAR_INDEX type INT4
    exporting
      !PAR_ITEM type ref to DATA
    raising
      /WATP/CX_BASE .
  methods AVVS_CHANGING
    importing
      !PAR_INDEX type INT4
    exporting
      !PAR_ITEM type ref to DATA
    raising
      /WATP/CX_BASE .
  methods AVKS_DELETE
    importing
      !PAR_INDEX type INT4
    raising
      /WATP/CX_BASE .
  methods AVVS_DELETE
    importing
      !PAR_INDEX type INT4
    raising
      /WATP/CX_BASE .
  methods IDENTS_DELETE
    importing
      !PAR_INDEX type INT4
    raising
      /WATP/CX_BASE .
  methods AVVS_INSERT
    exporting
      !PAR_INDEX type I
      !PAR_ITEM type ref to DATA
    raising
      /WATP/CX_BASE .
  methods AVKS_INSERT
    exporting
      !PAR_INDEX type I
      !PAR_ITEM type ref to DATA
    raising
      /WATP/CX_BASE .
  methods IDENTS_INSERT
    exporting
      !PAR_INDEX type I
      !PAR_ITEM type ref to DATA
    raising
      /WATP/CX_BASE .
  methods INIT_IDENTS
    importing
      !PAR_STRUCTURE_NAME type CHAR30 default '/WATP/TSRCPLID'
      !PAR_GROUP_NAME type CHAR30 default '*' .
  methods INIT_AVKS
    importing
      !PAR_STRUCTURE_NAME type CHAR30 default '/WATP/SWDPLANAVK'
      !PAR_GROUP_NAME type CHAR30 default '/WATP/TWDPLANAVK' .
  methods INIT_AVVS
    importing
      !PAR_STRUCTURE_NAME type CHAR30 default '/WATP/SWDPLANAVV'
      !PAR_GROUP_NAME type CHAR30 default '/WATP/TWDPLANAVV' .

  methods KEYEXISTS
    redefinition .
protected section.
*"* protected components of class /WATP/CL_BO_HW_WDPLANT
*"* do not include other source files here!!!

  data FLASTLOADEDTPLNR type TPLNR .

  methods LOAD_RELATION_ADRTP
    raising
      /WATP/CX_BASE .
  methods SAVE_RELATION_ADRTP
    raising
      /WATP/CX_BASE .
  methods CHECK_IDENTS
    raising
      /WATP/CX_BASE .
  methods CHECK_IDENTSRECORD
    importing
      !PAR_RECORD type ref to DATA
    raising
      /WATP/CX_BASE .
  methods LOAD_IDENTS
    raising
      /WATP/CX_BASE .
  methods LOAD_AVKS
    raising
      /WATP/CX_BASE .
  methods LOAD_AVVS
    raising
      /WATP/CX_BASE .
  methods SAVE_AVKS
    raising
      /WATP/CX_BASE .
  methods SAVE_AVVS
    raising
      /WATP/CX_BASE .
  methods SAVE_IDENTS
    raising
      /WATP/CX_BASE .

  methods CHECK_DATA_SPEC
    redefinition .
  methods LOAD_DATA_SPEC
    redefinition .
  methods LOAD_DETAILS
    redefinition .
  methods LOAD_RELATIONS
    redefinition .
  methods REGISTER_RELATIONS
    redefinition .
  methods SAVE_DETAILS
    redefinition .
  methods SAVE_RELATIONS
    redefinition .
  methods REGISTER_DETAILS
    redefinition .
private section.
*"* private components of class /WATP/CL_BO_HW_WDPLANT
*"* do not include other source files here!!!
ENDCLASS.



CLASS /WATP/CL_BO_HW_WDPLANT IMPLEMENTATION.


method AVKS_CHANGED .
  UPDATE_DETAILRECORD(
    PAR_NAME  = CDETAIL_AVKS
    PAR_INDEX = PAR_INDEX
  ).
endmethod.


method AVKS_CHANGING .
  data:
    LRECORD type ref to /WATP/SBO_DETAILRECORD.

  GET_DETAILRECORD(
    exporting
      PAR_NAME   = CDETAIL_AVKS
      PAR_INDEX  = PAR_INDEX
    importing
      PAR_RECORD = LRECORD
  ).

  if not LRECORD is initial.
    PAR_ITEM = LRECORD->STRUCTREF.
  else.
    clear PAR_ITEM.
  endif.
endmethod.


method AVKS_DELETE .
  DELETE_DETAILRECORD(
    PAR_NAME  = CDETAIL_AVKS
    PAR_INDEX = PAR_INDEX
  ).
endmethod.


method AVKS_INSERT .
  data:
    LDETAIL type /WATP/TWDPLANAVK,
    LRECORD type ref to /WATP/SBO_DETAILRECORD.

  LDETAIL-WDPLANTNR = DATAREF->WDPLANTNR.

  PAR_INDEX = INSERT_DETAILRECORD(
    PAR_NAME        = CDETAIL_AVKS
    PAR_STRUCTURE   = LDETAIL
  ).

  GET_DETAILRECORD(
    exporting
      PAR_NAME      = CDETAIL_AVKS
      PAR_INDEX     = PAR_INDEX
    importing
      PAR_RECORD    = LRECORD
  ).

  PAR_ITEM = LRECORD->STRUCTREF.
endmethod.


method AVVS_CHANGED .
  UPDATE_DETAILRECORD(
    PAR_NAME  = CDETAIL_AVVS
    PAR_INDEX = PAR_INDEX
  ).
endmethod.


method AVVS_CHANGING .
  data:
    LRECORD type ref to /WATP/SBO_DETAILRECORD.

  GET_DETAILRECORD(
    exporting
      PAR_NAME   = CDETAIL_AVVS
      PAR_INDEX  = PAR_INDEX
    importing
      PAR_RECORD = LRECORD
  ).

  if not LRECORD is initial.
    PAR_ITEM = LRECORD->STRUCTREF.
  else.
    clear PAR_ITEM.
  endif.
endmethod.


method AVVS_DELETE .
  DELETE_DETAILRECORD(
    PAR_NAME  = CDETAIL_AVVS
    PAR_INDEX = PAR_INDEX
  ).
endmethod.


method AVVS_INSERT .
  data:
    LDETAIL type /WATP/TWDPLANAVV,
    LRECORD type ref to /WATP/SBO_DETAILRECORD.

  LDETAIL-WDPLANTNR = DATAREF->WDPLANTNR.

  PAR_INDEX = INSERT_DETAILRECORD(
    PAR_NAME        = CDETAIL_AVVS
    PAR_STRUCTURE   = LDETAIL
  ).

  GET_DETAILRECORD(
    exporting
      PAR_NAME      = CDETAIL_AVVS
      PAR_INDEX     = PAR_INDEX
    importing
      PAR_RECORD    = LRECORD
  ).

  PAR_ITEM = LRECORD->STRUCTREF.
endmethod.


method CHECK_AVV_DELIVERY_STATIC .
  data:
    LWDPLANTNR type EWAEL_WDPLANTNR,
    LAVVS type table of /WATP/DAVVCODE.

  RAISE_BO_ARB_DATA.

  if PAR_AVVCODE is not initial.
    select AVVCODE into table LAVVS from /WATP/TWDPLANAVV where WDPLANTNR = PAR_WDPLANTNR.
    if LAVVS is not initial.
      read table LAVVS with key TABLE_LINE = PAR_AVVCODE transporting no fields.
      if SY-SUBRC <> 0.
        write PAR_WDPLANTNR to LWDPLANTNR.
        RAISE_BO_ARB_STATIC_TEXTS DELIVERY_LOCK_WDPLANT_AVV LWDPLANTNR PAR_AVVCODE ''.
      endif.
    endif.
  endif.

endmethod.


method CHECK_DATA_SPEC .
  SUPER->CHECK_DATA_SPEC( ).
  CHECK_IDENTS( ).
endmethod.


method CHECK_IDENTS .

  RAISE_BO_ARB_DATA.

  types: TREFWDPLANTID type ref to /WATP/TWDPLANTID.

  data:
     LVDETAIL type TREFWDPLANTID,
     LVMDETAIL type TREFWDPLANTID,
     LVDETAILS type table of TREFWDPLANTID,
     LVMDETAILS type table of TREFWDPLANTID,
     LRECORD type ref to /WATP/SBO_DETAILRECORD,
     LDETAIL type ref to TDETAILENTRY.

  field-symbols:
    <LPOSITION> type /WATP/TWDPLANTID.

  read table FDETAILS reference into LDETAIL
    with key NAME = CDETAIL_IDENTS.

  if SY-SUBRC is initial.
* Einträge prüfen
    loop at LDETAIL->DETAILRECORDS reference into LRECORD.
      if LRECORD->STATUS <> 'D'.
        assign LRECORD->DETAILREF->* to <LPOSITION>.
        BEGIN_EX_BO.
          CHECK_IDENTSRECORD( PAR_RECORD = LRECORD->DETAILREF ).
          get reference of <LPOSITION> into LVDETAIL.
          append LVDETAIL to LVDETAILS.
          if LRECORD->STATUS = 'U' or LRECORD->STATUS = 'I'.
            append LVDETAIL to LVMDETAILS.
          endif.
        END_EX_BO_RERAISE.
      endif.
    endloop.

    loop at LVMDETAILS into LVMDETAIL.
      loop at LVDETAILS into LVDETAIL.
        if LVDETAIL <> LVMDETAIL
        and LVDETAIL->IDENTTYPE = LVMDETAIL->IDENTTYPE
        and LVDETAIL->COUNTRY = LVMDETAIL->COUNTRY
        and LVDETAIL->REGION = LVMDETAIL->REGION
        and LVMDETAIL->VALID_DATE_TO >= LVDETAIL->VALID_DATE_FROM
        and LVDETAIL->VALID_DATE_TO >= LVMDETAIL->VALID_DATE_FROM.
          RAISE_BO_ARB_TEXTS IDENT_TIMEFRAME_OVERLAP LVDETAIL->IDNUMBER LVMDETAIL->IDNUMBER ''.
        endif.
      endloop.
    endloop.

  endif.
endmethod.


method CHECK_IDENTSRECORD .
  data: LTEXT type string.

  field-symbols:
    <LPOSITION> type /WATP/TWDPLANTID.

  assign PAR_RECORD->* to <LPOSITION>.
  if <LPOSITION>-VALID_DATE_TO < <LPOSITION>-VALID_DATE_FROM.
    LTEXT = <LPOSITION>-IDNUMBER.
    RAISE_BO_CHECK_DETAIL /WATP/CX_BO_ARB VALID_FROM_GT_VALID_TO IDENTS LTEXT.
  endif.


endmethod.


method IDENTS_CHANGED .
  UPDATE_DETAILRECORD(
    PAR_NAME  = CDETAIL_IDENTS
    PAR_INDEX = PAR_INDEX
  ).
endmethod.


method IDENTS_CHANGING .
  data:
    LRECORD type ref to /WATP/SBO_DETAILRECORD.

  GET_DETAILRECORD(
    exporting
      PAR_NAME = CDETAIL_IDENTS
      PAR_INDEX = PAR_INDEX
    importing
      PAR_RECORD = LRECORD
  ).

  if not LRECORD is initial.
    PAR_ITEM = LRECORD->STRUCTREF.
  else.
    clear PAR_ITEM.
  endif.
endmethod.


method IDENTS_DELETE .
  DELETE_DETAILRECORD(
    PAR_NAME  = CDETAIL_IDENTS
    PAR_INDEX = PAR_INDEX
  ).
endmethod.


method IDENTS_INSERT .
  data:
    LDETAIL type /WATP/TWDPLANTID,
    LRECORD type ref to /WATP/SBO_DETAILRECORD.

  LDETAIL-WDPLANTNR = DATAREF->WDPLANTNR.
  LDETAIL-ENTRY_DATE = SY-DATUM.

  PAR_INDEX = INSERT_DETAILRECORD(
    PAR_NAME      = CDETAIL_IDENTS
    PAR_STRUCTURE = LDETAIL
  ).

  GET_DETAILRECORD(
    exporting
      PAR_NAME      = CDETAIL_IDENTS
      PAR_INDEX     = PAR_INDEX
    importing
      PAR_RECORD    = LRECORD
  ).

  PAR_ITEM = LRECORD->STRUCTREF.
endmethod.


method INIT_AVKS .
  INIT_DETAIL(
    PAR_NAME           = CDETAIL_AVKS
    PAR_STRUCTURE_NAME = PAR_STRUCTURE_NAME
    PAR_GROUP_NAME     = PAR_GROUP_NAME
    PAR_MSGID          = '/WATP/NARB'
    PAR_MSGNO          = '120'
    PAR_KEYFIELD       = 'AVKCODE,SPEZIFIKATION' ).
endmethod.


method INIT_AVVS .
  INIT_DETAIL(
    PAR_NAME           = CDETAIL_AVVS
    PAR_STRUCTURE_NAME = PAR_STRUCTURE_NAME
    PAR_GROUP_NAME     = PAR_GROUP_NAME
    PAR_MSGID          = '/WATP/NARB'
    PAR_MSGNO          = '000'
    PAR_KEYFIELD       = 'AVVCODE' ).
endmethod.


method INIT_IDENTS .
  INIT_DETAIL(
    PAR_NAME           = CDETAIL_IDENTS
    PAR_STRUCTURE_NAME = PAR_STRUCTURE_NAME
    PAR_GROUP_NAME     = PAR_GROUP_NAME
    PAR_MSGID          = '/WATP/NARB'
    PAR_MSGNO          = '053'
    PAR_KEYFIELD       = 'IDNUMBER' ).
endmethod.


method INSERT_PARENT_IDENTS .
  data:
    LBUT0ID      type BUT0ID,
    LTBUT0ID     type table of BUT0ID,
    LDETAIL      type /WATP/TWDPLANTID,
*   LRECORD      type ref to /WATP/SBO_DETAILRECORD,
    LT_PTNRTYP   type table of /WATP/TTPTNRTYP.

  DELETE_DETAILRECORDS( PAR_NAME = CDETAIL_IDENTS ).

  select * into table LT_PTNRTYP from /WATP/TTPTNRTYP WHERE PTNRTYPE = 'ENT' or PTNRTYPE = 'NGS'.
  if LT_PTNRTYP is not initial.
   select * into table LTBUT0ID from BUT0ID
     FOR ALL ENTRIES IN LT_PTNRTYP
     where partner = DATAREF->/WATP/PTR_ENT
     and type = LT_PTNRTYP-IDENTTYPE.
  endif.
*  select * into table LTBUT0ID from BUT0ID where partner = DATAREF->/WATP/PTR_ENT
*  and TYPE IN ( SELECT IDENTTYPE FROM /WATP/TTPTNRTYP WHERE PTNRTYPE = 'ENT' or PTNRTYPE = 'NGS' ).

  loop at LTBUT0ID into LBUT0ID.
    LDETAIL-WDPLANTNR = DATAREF->WDPLANTNR.
    LDETAIL-IDENTTYPE = LBUT0ID-TYPE.
    LDETAIL-IDNUMBER = LBUT0ID-IDNUMBER.

    if LBUT0ID-VALID_DATE_FROM is initial.
      LDETAIL-VALID_DATE_FROM = SY-DATUM.
    else.
      LDETAIL-VALID_DATE_FROM = LBUT0ID-VALID_DATE_FROM.
    endif.

    if LBUT0ID-VALID_DATE_TO is initial.
      write '99991231' to LDETAIL-VALID_DATE_TO.
    else.
      LDETAIL-VALID_DATE_TO = LBUT0ID-VALID_DATE_TO.
    endif.

    LDETAIL-COUNTRY = LBUT0ID-COUNTRY.
    LDETAIL-REGION = LBUT0ID-REGION.

    INSERT_DETAILRECORD(
      PAR_NAME      = CDETAIL_IDENTS
      PAR_STRUCTURE = LDETAIL
    ).
  endloop.

endmethod.


method KEYEXISTS.
  clear PAR_OK.
endmethod.


method LOAD_AVKS .
  BO_LOAD_DETAIL_STRUCT AVKS /WATP/TWDPLANAVK 'WDPLANTNR = DATAREF->WDPLANTNR' /WATP/SWDPLANAVK.

*  data:
*    LDETAILS type standard table of /WATP/TWDPLANAVK,
*    LDETAIL type ref to /WATP/TWDPLANAVK.
*
*  select * from /WATP/TWDPLANAVK into table LDETAILS where WDPLANTNR = DATAREF->WDPLANTNR.
*  loop at LDETAILS reference into LDETAIL.
*    APPEND_DETAILRECORD( PAR_NAME = CDETAIL_AVKS PAR_STRUCTURE = LDETAIL->* ).
*  endloop.
endmethod.


method LOAD_AVVS .
  BO_LOAD_DETAIL_STRUCT AVVS /WATP/TWDPLANAVV 'WDPLANTNR = DATAREF->WDPLANTNR' /WATP/SWDPLANAVV.

*  data:
*    LDETAILS type standard table of /WATP/TWDPLANAVV,
*    LDETAIL type ref to /WATP/TWDPLANAVV.
*
*  select * from /WATP/TWDPLANAVV
*    into table LDETAILS
*    where WDPLANTNR = DATAREF->WDPLANTNR.
*
*  loop at LDETAILS reference into LDETAIL.
*    APPEND_DETAILRECORD(
*      PAR_NAME      = CDETAIL_AVVS
*      PAR_STRUCTURE = LDETAIL->* ).
*  endloop.
endmethod.


method LOAD_DATA_SPEC .
  data:
    L_DATA type EWA_EL_WDPLANT.

  move-corresponding DATAREF->* to L_DATA.
  CLEAR_DATA( ).
  move-corresponding L_DATA to DATAREF->*.
endmethod.


method LOAD_DETAILS .
  SUPER->LOAD_DETAILS( ).
  if ISDETAILUSED( CDETAIL_IDENTS ) is not initial.
    LOAD_IDENTS( ).
  endif.
  if ISDETAILUSED( CDETAIL_AVVS ) is not initial.
    LOAD_AVVS( ).
  endif.
  if ISDETAILUSED( CDETAIL_AVKS ) is not initial.
    LOAD_AVKS( ).
  endif.
endmethod.


method LOAD_IDENTS .
  data:
    LDETAILS type standard table of /WATP/TWDPLANTID,
    LDETAIL type ref to /WATP/TWDPLANTID.

  select * from /WATP/TWDPLANTID
    into table LDETAILS
    where WDPLANTNR = DATAREF->WDPLANTNR.

  loop at LDETAILS reference into LDETAIL.
    APPEND_DETAILRECORD(
      PAR_NAME      = CDETAIL_IDENTS
      PAR_STRUCTURE = LDETAIL->* ).
  endloop.
endmethod.


method LOAD_RELATIONS .
  SUPER->LOAD_RELATIONS( ).
  if ISRELATIONUSED( C_RELATION_ADRTP ) is not initial.
    LOAD_RELATION_ADRTP( ).
  endif.
endmethod.


method LOAD_RELATION_ADRTP .
  FLASTLOADEDTPLNR = DATAREF->TPLNR.
  if DATAREF->TPLNR is not initial.
    DATAREF_ADRTP->* = /WATP/CL_BO_IFLOT=>READ_TPLADDRESS(
      PAR_TPLNR = DATAREF->TPLNR
    ).
    if DATAREF_ADRTP->ADDRNUMBER is initial.
      clear DATAREF_ADRTP->*.
    endif.
  else.
    clear DATAREF_ADRTP->*.
  endif.
endmethod.


  method REGISTER_DETAILS.
    SUPER->REGISTER_DETAILS( ).
    REGISTER_DETAIL(
      PAR_NAME = CDETAIL_AVKS
      PAR_STRUCTURE_NAME = '/WATP/SWDPLANAVK'
      PAR_KEYFIELD = 'AVKCODE,SPEZIFIKATION'
      PAR_USED = 'X'
    ).
    REGISTER_DETAIL(
      PAR_NAME = CDETAIL_AVVS
      PAR_STRUCTURE_NAME = '/WATP/SWDPLANAVV'
      PAR_KEYFIELD = 'AVVCODE'
      PAR_USED = 'X'
    ).
    REGISTER_DETAIL(
      PAR_NAME = CDETAIL_IDENTS
      PAR_STRUCTURE_NAME = '/WATP/TWDPLANTID'
      PAR_KEYFIELD = 'IDNUMBER'
      PAR_USED = 'X'
    ).
  endmethod.


method REGISTER_RELATIONS .
  SUPER->REGISTER_RELATIONS( ).
  REGISTER_RELATION( PAR_NAME = C_RELATION_ADRTP ).
endmethod.


method SAVE_AVKS .
  BO_SAVE_DETAIL_STRUCT AVKS /WATP/TWDPLANAVK /WATP/TWDPLANAVK.

*  data:
*    LRECORD type ref to /WATP/SBO_DETAILRECORD,
*    LDETAIL type ref to TDETAILENTRY.
*
*  field-symbols:
*    <LPOSITION> type /WATP/TWDPLANAVK.
*
*  read table FDETAILS reference into LDETAIL with key NAME = CDETAIL_AVKS.
*
*  if SY-SUBRC is initial.
*    loop at LDETAIL->DETAILRECORDS reference into LRECORD.
*      assign LRECORD->DETAILREF->* to <LPOSITION>.
*      case LRECORD->STATUS.
*        when 'I'. insert /WATP/TWDPLANAVK from <LPOSITION>.
*        when 'U'. update /WATP/TWDPLANAVK from <LPOSITION>.
*        when 'D'. delete /WATP/TWDPLANAVK from <LPOSITION>.
*      endcase.
*      if SY-SUBRC is not initial.
*        RAISE_BO /WATP/CX_BO_BASE UPDATE_FAILED.
*      endif.
*    endloop.
*  endif.
endmethod.


method SAVE_AVVS .
  BO_SAVE_DETAIL_STRUCT AVVS /WATP/TWDPLANAVV /WATP/TWDPLANAVV.

*  data:
*    LRECORD type ref to /WATP/SBO_DETAILRECORD,
*    LDETAIL type ref to TDETAILENTRY.
*
*  field-symbols:
*    <LPOSITION> type /WATP/TWDPLANAVV.
*
*  read table FDETAILS reference into LDETAIL with key NAME = CDETAIL_AVVS.
*
*  if SY-SUBRC is initial.
*    loop at LDETAIL->DETAILRECORDS reference into LRECORD.
*      assign LRECORD->DETAILREF->* to <LPOSITION>.
*      case LRECORD->STATUS.
*        when 'I'. insert /WATP/TWDPLANAVV from <LPOSITION>.
*        when 'U'. update /WATP/TWDPLANAVV from <LPOSITION>.
*        when 'D'. delete /WATP/TWDPLANAVV from <LPOSITION>.
*      endcase.
*      if SY-SUBRC is not initial.
*        RAISE_BO /WATP/CX_BO_BASE UPDATE_FAILED.
*      endif.
*    endloop.
*  endif.
endmethod.


method SAVE_DETAILS .
  SUPER->SAVE_DETAILS( ).
  if ISDETAILUSED( CDETAIL_IDENTS ) is not initial.
    SAVE_IDENTS( ).
  endif.
  if ISDETAILUSED( CDETAIL_AVVS ) is not initial.
    SAVE_AVVS( ).
  endif.
  if ISDETAILUSED( CDETAIL_AVKS ) is not initial.
    SAVE_AVKS( ).
  endif.
endmethod.


method SAVE_IDENTS .

  BO_SAVE_DETAIL IDENTS /WATP/TWDPLANTID.

*  data:
*    LRECORD type ref to /WATP/SBO_DETAILRECORD,
*    LDETAIL type ref to TDETAILENTRY.
*
*  field-symbols:
*    <LPOSITION> type /WATP/TWDPLANTID.
*
*  read table FDETAILS reference into LDETAIL
*    with key NAME = CDETAIL_IDENTS.
*
*  if SY-SUBRC is initial.
*    loop at LDETAIL->DETAILRECORDS reference into LRECORD.
*      assign LRECORD->DETAILREF->* to <LPOSITION>.
*      case LRECORD->STATUS.
*        when 'I'. insert /WATP/TWDPLANTID from <LPOSITION>.
*        when 'U'. update /WATP/TWDPLANTID from <LPOSITION>.
*        when 'D'. delete /WATP/TWDPLANTID from <LPOSITION>.
*      endcase.
*      if SY-SUBRC is not initial.
*        RAISE_BO /WATP/CX_BO_BASE UPDATE_FAILED.
*      endif.
*    endloop.
*  endif.
endmethod.


method SAVE_RELATIONS .
  SUPER->SAVE_RELATIONS( ).
  if ISRELATIONUSED( C_RELATION_ADRTP ) is not initial.
    SAVE_RELATION_ADRTP( ).
  endif.
endmethod.


method SAVE_RELATION_ADRTP .
  data:
    LADDRESS type /WATP/SADDRESS.
  if FLASTLOADEDTPLNR = DATAREF->TPLNR.
    if DATAREF_ADRTP->ADDRNUMBER is not initial.
      LADDRESS = /WATP/CL_BO_ADRC=>READ_ADDRESS(
        PAR_ADDRNR = DATAREF_ADRTP->ADDRNUMBER
      ).
      if LADDRESS-ADDRNUMBER is not initial.
        if DATAREF_ADRTP->* <> LADDRESS.
          /WATP/CL_BO_ADRC=>ADDR_CHANGE(
            PAR_ADDRESS = DATAREF_ADRTP->*
          ).
        endif.
      endif.
    endif.
  endif.
endmethod.


method VALIDATE_RELATION_ADRTP .
  LOAD_RELATION_ADRTP( ).
endmethod.
ENDCLASS.
