class ZCL_WR_WAA_TA_SELFWEIGHING definition
  public
  inheriting from CL_EEWA_TRANSACTION
  create public .

public section.

  types:
    begin of STY_WEIGHING_REQUEST,
      ORDERID	    type EWA_ORDER_HEAD-HOBJNR,
      ORDERITEMID	type EWA_ORDER_OBJECT-POBJNR,
      WASTEID	    type WASTE_TYPE,
      VEHICLEID	  type EQUNR,
      WEIGHINGID  type EWAWA_WEIGHINGNR, "for tara weíghing
      PROCESSTYPE	type EWAEL_PROCESSTYPE,
      DELIVERYTYPE type EWADWA_WEIGHPROCINTEXT, "optional default 'I'
      FACILITYID  type EWAEL_WDPLANTNR,
      DEVICEGROUP type EWAWA_DEVICEGROUPNR,
      SIMULATION  type KENNZX,
    end of STY_WEIGHING_REQUEST .

  data GS_RESULT_DATA type ZWR_SWAA_SELFWEIGH_RESULT .
  constants C_TA_NAME type EWAEL_OBJTRANSACTION value 'ZWR_SELFWEIGHING' ##NO_TEXT.
  data GS_PROCESS_DATA type STY_WEIGHING_REQUEST .

  class-methods CL_GET_OPEN_WEIGHPROC
    importing
      !IV_POBJNR type J_OBJNR
    returning
      value(RV_WEIGHINGNR) type EWAWA_WEIGHINGNR .
  class-methods CL_WEIGHING
    importing
      !IV_POBJNR type J_OBJNR
      !IV_LICENCE_PLATE type LICENSE_NUM optional
      !IV_SIMULATION type ABAP_BOOL default ABAP_FALSE
      !IV_USER type XUBNAME default SY-UNAME
      !IV_COMMIT type ABAP_BOOL default ABAP_FALSE
      !IV_WDPLANT type EWAEL_WDPLANTNR optional
      !IV_DEVICEGROUPNR type EWAWA_DEVICEGROUPNR optional
    exporting
      !IV_WEIGHING_RESULT type ZWR_SWAA_SELFWEIGH_RESULT
    raising
      CX_EEWA_BASE .
  methods ZZ_BOOK_WEIGHING
    raising
      CX_EEWA_BASE .
  methods ZZ_CHECK_BOOK_WEIGHING
    raising
      CX_EEWA_BASE .
protected section.

  methods ZZ_DO_FIELDMAPPING
    importing
      !IO_BO_HEAD type ref to CL_EEWA_BO_MULTIWEIGHPROCHEAD
      !IO_BO_ITEM type ref to CL_EEWA_BO_MULTIWEIGHPROCITEM
      !IT_MODIFIEDFIELDS type EWAPDDICFIELDNAMES
    raising
      CX_EEWA_BASE .
  methods ZZ_DO_FILL_ARB_DATA
    importing
      !IO_BO_HEAD type ref to CL_EEWA_BO_MULTIWEIGHPROCHEAD
      !IO_BO_ITEM type ref to CL_EEWA_BO_MULTIWEIGHPROCITEM
    raising
      CX_EEWA_BASE .
  methods ZZ_DO_FILL_DEFAULTS
    importing
      !IO_BO_HEAD type ref to CL_EEWA_BO_MULTIWEIGHPROCHEAD
      !IO_BO_ITEM type ref to CL_EEWA_BO_MULTIWEIGHPROCITEM
    raising
      CX_EEWA_BASE .
  methods ZZ_DO_PROCESS_WEIGHING .
  methods ZZ_DO_WEIGHING
    importing
      !IV_SIMULATION type ABAP_BOOL default ABAP_FALSE
    raising
      CX_EEWA_BASE .
  methods ZZ_GET_MODIFIED_FIELDS
    importing
      !IS_SRC_HEAD type EWASWA_MULTIWEIGHPROC_HEAD
      !IS_DST_HEAD type EWASWA_MULTIWEIGHPROC_HEAD
      !IS_SRC_ITEM type EWASWA_MULTIWEIGHPROC_ITEM
      !IS_DST_ITEM type EWASWA_MULTIWEIGHPROC_ITEM
    exporting
      !ET_MODIFIED_FIELDS type EWAPDDICFIELDNAMES .

  methods CALL_METHOD
    redefinition .
private section.
ENDCLASS.



CLASS ZCL_WR_WAA_TA_SELFWEIGHING IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->CALL_METHOD
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_PARAMS                     TYPE        ABAP_PARMBIND_TAB(optional)
* | [--->] PAR_NAME                       TYPE        CSEQUENCE
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method CALL_METHOD.
    TA_CALL_METHOD.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_WAA_TA_SELFWEIGHING=>CL_GET_OPEN_WEIGHPROC
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_POBJNR                      TYPE        J_OBJNR
* | [<-()] RV_WEIGHINGNR                  TYPE        EWAWA_WEIGHINGNR
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method CL_GET_OPEN_WEIGHPROC.
    types:
      begin of TY_WEIGHINGLEAD,
        WEIGHINGNR_LEAD type EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD,
      end of TY_WEIGHINGLEAD.
    data:
      LV_GROSSID      type EWA_WA_WEIGHPROC-GROSS_ID,
      LV_TAREID       type EWA_WA_WEIGHPROC-TARE_ID,
      LV_WEIGHINGLEAD type EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD,
      LV_MAX          type INT4 value 1,
      LT_WEIGHINGNR   type standard table of TY_WEIGHINGLEAD.

    clear RV_WEIGHINGNR.
    if IV_POBJNR is not initial.
      DATA_BO_RANGE X.
      SET_BO_RANGE X POBJNR IV_POBJNR J_OBJNR.
      SET_BO_RANGE_CMPLT X GROSS_ID LV_GROSSID EWA_WA_WEIGHPROC-GROSS_ID I NE.
      SET_BO_RANGE_CMPLT X TAREID   LV_TAREID EWA_WA_WEIGHPROC-TARE_ID I EQ.
      SET_BO_RANGE_CMPLT X WEIGHINGNR_LEAD LV_WEIGHINGLEAD EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD I NE.

      CL_EEWA_OR_WEIGHINGPROCESS=>CL_READ(
        exporting
          PAR_OBJTYPE = CL_EEWA_BO_WEIGHINGPROCESS=>COT_WEIGHINGPROCESS
          PAR_DOMAIN  = 'OPEN'
          PAR_RANGES  = LRANGEXS
          PAR_MAXROWS = LV_MAX
        importing
          PAR_TABLE   = LT_WEIGHINGNR
      ).
      if LINES( LT_WEIGHINGNR ) > 0.
        RV_WEIGHINGNR = LT_WEIGHINGNR[ 1 ]-WEIGHINGNR_LEAD.
      endif.
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_WAA_TA_SELFWEIGHING=>CL_WEIGHING
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_POBJNR                      TYPE        J_OBJNR
* | [--->] IV_LICENCE_PLATE               TYPE        LICENSE_NUM(optional)
* | [--->] IV_SIMULATION                  TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [--->] IV_USER                        TYPE        XUBNAME (default =SY-UNAME)
* | [--->] IV_COMMIT                      TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [--->] IV_WDPLANT                     TYPE        EWAEL_WDPLANTNR(optional)
* | [--->] IV_DEVICEGROUPNR               TYPE        EWAWA_DEVICEGROUPNR(optional)
* | [<---] IV_WEIGHING_RESULT             TYPE        ZWR_SWAA_SELFWEIGH_RESULT
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method CL_WEIGHING.
    data:
      LS_PROCESS        type ZCL_WR_WAA_TA_SELFWEIGHING=>STY_WEIGHING_REQUEST,
      LS_PROCESS_RESULT type ZWR_SWAA_SELFWEIGH_RESULT.

    clear:
      IV_WEIGHING_RESULT.

    LS_PROCESS-DEVICEGROUP = IV_DEVICEGROUPNR.
    LS_PROCESS-FACILITYID  = IV_WDPLANT.

    if LS_PROCESS-DEVICEGROUP is initial.
      select single *
        into @data(LS_HW_PROFILE)
        from EEWA_SCALE_USER
        where XUSER = @IV_USER.
      if sy-subrc is initial.
        LS_PROCESS-DEVICEGROUP = LS_HW_PROFILE-DEVICEGROUPNR.
        LS_PROCESS-FACILITYID  = LS_HW_PROFILE-WDPLANT.
      endif.
    endif.

    if LS_PROCESS-DEVICEGROUP is not initial.
      LS_PROCESS-WEIGHINGID = CL_GET_OPEN_WEIGHPROC(
        exporting
          IV_POBJNR   = IV_POBJNR
      ).
      LS_PROCESS-PROCESSTYPE = CL_EEWA_BO_WDORDERWEIGH=>CPROCESSTYPE_INBOUND.
      LS_PROCESS-SIMULATION  = IV_SIMULATION.
      LS_PROCESS-DELIVERYTYPE = CL_EEWA_BO_MULTIWEIGHPROCHEAD=>CDT_EXTERNAL.

      TA_BEGIN_NAMED_VAR TA ZCL_WR_WAA_TA_SELFWEIGHING=>C_TA_NAME.
      TA_DO_RAISE_EXCEPTION TA.
      if IV_COMMIT = ABAP_TRUE.
        TA_DO_COMMIT TA.
      else.
        TA_NO_COMMIT TA.
      endif.
      TA_SET_PARAM TA GS_PROCESS_DATA LS_PROCESS.
      TA_EXECUTE TA ZZ_BOOK_WEIGHING.
      TA_GET_PARAM TA GS_RESULT_DATA LS_PROCESS_RESULT.
      TA_END TA.

    endif.
    IV_WEIGHING_RESULT = LS_PROCESS_RESULT.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_BOOK_WEIGHING
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_BOOK_WEIGHING.
       BEGIN_UPDATE( ).
    try.
       CHECK( PAR_NAME = 'ZZ_CHECK_BOOK_WEIGHING' ).
       CALL( PAR_NAME = 'ZZ_DO_PROCESS_WEIGHING' ).
     cleanup .
       END_UPDATE( PAR_FAILED = 'X' ).
    endtry.
   END_UPDATE( ).
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_CHECK_BOOK_WEIGHING
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_CHECK_BOOK_WEIGHING.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_DO_FIELDMAPPING
* +-------------------------------------------------------------------------------------------------+
* | [--->] IO_BO_HEAD                     TYPE REF TO CL_EEWA_BO_MULTIWEIGHPROCHEAD
* | [--->] IO_BO_ITEM                     TYPE REF TO CL_EEWA_BO_MULTIWEIGHPROCITEM
* | [--->] IT_MODIFIEDFIELDS              TYPE        EWAPDDICFIELDNAMES
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_DO_FIELDMAPPING.

    types:
      begin of __TMAPITM.
        include type EWASWA_MULTIWEIGHPROC_HD_EXCL  as EWASWA_MULTIWEIGHPROC_HD_EXCL.
        include type EWASWA_MULTIWEIGHPROC_ITM_EXCL as EWASWA_MULTIWEIGHPROC_ITM_EXCL.
    types: WEIGHINGNR type EWAWA_WEIGHINGNR, "To react on modify weighingnr
      end of __TMAPITM.


    data: LO_FIELDTRANSPORT type ref to CL_EEWA_BEHAVIOR_FM_LIST,
          LO_RESOLVER       type ref to LCL_RESOLVER,
          LS_MAP            type        __TMAPITM.

    create object LO_RESOLVER.
    create object LO_FIELDTRANSPORT.

    LO_FIELDTRANSPORT->INIT(
      PAR_APPNAME = 'EWAMULTIWEIGHPROCFAST'
      PAR_RESOLVER = LO_RESOLVER
      PAR_MAPPERCLASS = 'ZCL_WR_WAA_FIELDMAPPER_MWGH'
    ).

    LO_RESOLVER->YY_SET_BO( IO_BO_HEAD ).

    LO_RESOLVER->YY_SET_CURRENT_ITEM( IO_BO_ITEM ).

    LS_MAP-EWASWA_MULTIWEIGHPROC_HD_EXCL  = IO_BO_HEAD->DATAREF->EWASWA_MULTIWEIGHPROC_HD_EXCL.
    LS_MAP-EWASWA_MULTIWEIGHPROC_ITM_EXCL = IO_BO_ITEM->DATAREF->EWASWA_MULTIWEIGHPROC_ITM_EXCL.
    LS_MAP-WEIGHINGNR                     = IO_BO_HEAD->DATAREF->WEIGHINGNR.

    LO_FIELDTRANSPORT->DO_MAP( exporting PAR_MODIFIEDFIELDS = IT_MODIFIEDFIELDS changing PAR_STRUCT = LS_MAP ).

    IO_BO_HEAD->DATAREF->EWASWA_MULTIWEIGHPROC_HD_EXCL   = LS_MAP-EWASWA_MULTIWEIGHPROC_HD_EXCL.
    IO_BO_ITEM->DATAREF->EWASWA_MULTIWEIGHPROC_ITM_EXCL  = LS_MAP-EWASWA_MULTIWEIGHPROC_ITM_EXCL.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_DO_FILL_ARB_DATA
* +-------------------------------------------------------------------------------------------------+
* | [--->] IO_BO_HEAD                     TYPE REF TO CL_EEWA_BO_MULTIWEIGHPROCHEAD
* | [--->] IO_BO_ITEM                     TYPE REF TO CL_EEWA_BO_MULTIWEIGHPROCITEM
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_DO_FILL_ARB_DATA.
    "for future cases
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_DO_FILL_DEFAULTS
* +-------------------------------------------------------------------------------------------------+
* | [--->] IO_BO_HEAD                     TYPE REF TO CL_EEWA_BO_MULTIWEIGHPROCHEAD
* | [--->] IO_BO_ITEM                     TYPE REF TO CL_EEWA_BO_MULTIWEIGHPROCITEM
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_DO_FILL_DEFAULTS.
    IO_BO_ITEM->DATAREF->BRUTTO_UNIT = 'TO'.
    IO_BO_ITEM->DATAREF->TARA_UNIT = 'TO'.

    if IO_BO_HEAD->DATAREF->SCHDATUM is not initial.
      IO_BO_ITEM->DATAREF->GROSS_WDATE = IO_BO_HEAD->DATAREF->SCHDATUM.
      IO_BO_ITEM->DATAREF->TARE_WDATE = IO_BO_HEAD->DATAREF->SCHDATUM.
    endif.
    if IO_BO_HEAD->DATAREF->SCHUZEIT is not initial.
      IO_BO_ITEM->DATAREF->GROSS_WTIME = IO_BO_HEAD->DATAREF->SCHUZEIT.
      IO_BO_ITEM->DATAREF->TARE_WTIME = IO_BO_HEAD->DATAREF->SCHUZEIT.
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_DO_PROCESS_WEIGHING
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_DO_PROCESS_WEIGHING.
    data:
          LT_TMP_RESULT type BAPIRETTAB.

    clear GS_RESULT_DATA.
    try.
      ZZ_DO_WEIGHING( IV_SIMULATION = GS_PROCESS_DATA-SIMULATION ).
      catch CX_EEWA_BASE into DATA(LO_LEX).
    endtry.

*    if FMSGLOG_CALLBACK->MSGLOG->EXISTS_LOG( ).
*      FMSGLOG_CALLBACK->MSGLOG->READ_INTO_BAPIRET( importing PAR_BAPIRET2 = LTMP_RESULT ).
*      loop at LTMP_RESULT reference into data(LRMSG).
*        DO_ADD_MESSAGE( PAR_TYPE = LRMSG->TYPE PAR_TEXT = conv STRING( LRMSG->MESSAGE ) ).
*      endloop.
*      FMSGLOG_CALLBACK->MSGLOG->CLEAR( ).
*    endif.

*    RESULT_DATA-MESSAGES = /UI2/CL_JSON=>SERIALIZE( DATA = FMESSAGES COMPRESS = ABAP_TRUE ).
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_DO_WEIGHING
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_SIMULATION                  TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_DO_WEIGHING.
    data:
      LO_EX             type ref to CX_EEWA_BASE,
      LT_MODIFIEDFIELDS type EWAPDDICFIELDNAMES,
      LS_ORDERDATA      type EWA_ORDER_OBJECT,
      LO_BO_HEAD        type ref to CL_EEWA_BO_MULTIWEIGHPROCHEAD,
      LO_BO_ITEM        type ref to CL_EEWA_BO_MULTIWEIGHPROCITEM,
      LS_HEAD_BASE_DATA type EWASWA_MULTIWEIGHPROC_HEAD,
      LS_ITEM_BASE_DATA type EWASWA_MULTIWEIGHPROC_ITEM.

    LO_BO_HEAD ?= CL_EEWA_BO_OBJTYPE=>CL_CREATE_BO( PAR_OBJTYPE = CL_EEWA_BO_MULTIWEIGHPROCHEAD=>COT_MULTIWEIGHPROCHEAD ).
    LO_BO_HEAD->INIT_DATA( ).
*  try.
    if GS_PROCESS_DATA-WEIGHINGID is not initial.
      LO_BO_HEAD->LOAD_DATA(
        exporting
          PAR_KEY = GS_PROCESS_DATA-WEIGHINGID
          PAR_LOCK = ABAP_TRUE
      ).
    else.
      LO_BO_HEAD->NEW_DATA( exporting PAR_LOCK = ABAP_TRUE ).
    endif.
    try.
        LO_BO_HEAD->ITEM_FIND(
          exporting
            PAR_KEY = LO_BO_HEAD->DATAREF->EWASWEIGHINGPROCESS_KEY
          importing
            PAR_WGHITEMBO = LO_BO_ITEM ).

        if LO_BO_ITEM is initial.
          CL_EEWA_MISC=>TODO( ).
*            create object LEX type YCX_SERVICE exporting TEXTID = YCX_SERVICE=>WEIGHING_FAILED.
*            DO_ADD_MESSAGE( PAR_TYPE = 'E' PAR_TEXT = LEX->GET_TEXT( ) ).
          return.
        endif.

        LO_BO_ITEM->SET_CALLBACK( PAR_INTERFACE = FCALLBACK ).

        ZZ_DO_FILL_DEFAULTS(
          IO_BO_HEAD = LO_BO_HEAD
          IO_BO_ITEM = LO_BO_ITEM
        ).

        if GS_PROCESS_DATA-PROCESSTYPE = 'E'.
          LO_BO_ITEM->DATAREF->ZZ_SW_GROSS = 'X'.
        else.
          LO_BO_ITEM->DATAREF->ZZ_SW_TARA = 'X'.
        endif.

        LS_HEAD_BASE_DATA = LO_BO_HEAD->DATAREF->*.
        LS_ITEM_BASE_DATA = LO_BO_ITEM->DATAREF->*.

        LO_BO_HEAD->DATAREF->DELIVERYTYPE = GS_PROCESS_DATA-DELIVERYTYPE.
        if LO_BO_HEAD->DATAREF->DELIVERYTYPE is initial.
          LO_BO_HEAD->DATAREF->DELIVERYTYPE = CL_EEWA_BO_MULTIWEIGHPROCHEAD=>CDT_INTERNAL.
        endif.

        select single LICENSE_NUM into LO_BO_HEAD->DATAREF->LICENSE_NUM
          from V_FLEET where EQUNR = GS_PROCESS_DATA-VEHICLEID. "#EC CI_NOORDER

        LO_BO_HEAD->DATAREF->PROCESSTYPE = GS_PROCESS_DATA-PROCESSTYPE.

        LO_BO_ITEM->BOOK_APPLY_DEVICEGROUP( PAR_DEVICEGROUP = GS_PROCESS_DATA-DEVICEGROUP PAR_FORCE = ABAP_TRUE   ).

        select single ORDERNR into LO_BO_ITEM->DATAREF->ORDERNR from EWA_ORDER_HEAD where HOBJNR = GS_PROCESS_DATA-ORDERID.

        if GS_PROCESS_DATA-ORDERITEMID is not initial.
          select single *                     "#EC CI_ALL_FIELDS_NEEDED
            into corresponding fields of LS_ORDERDATA
            from EWA_ORDER_OBJECT as ITEM
            where ITEM~POBJNR = GS_PROCESS_DATA-ORDERITEMID.

          LO_BO_HEAD->DATAREF->KUNNR = LS_ORDERDATA-KUNNR.

          LO_BO_ITEM->DATAREF->POBJNR = LS_ORDERDATA-POBJNR.

          if LS_ORDERDATA-/WATP/NOTEINTNR is not initial.
            LO_BO_ITEM->DATAREF->/WATP/USE_NOTE = ABAP_TRUE.
            LO_BO_ITEM->DATAREF->/WATP/NOTEINTNR = LS_ORDERDATA-/WATP/NOTEINTNR.
            LO_BO_ITEM->DATAREF->/WATP/NOTENR = LS_ORDERDATA-/WATP/NOTENR.
            LO_BO_ITEM->DATAREF->/WATP/NWINTNR = LS_ORDERDATA-/WATP/NWINTNR.
            LO_BO_ITEM->DATAREF->/WATP/AVVCODE = LS_ORDERDATA-/WATP/AVVCODE.
          endif.
        endif.

        ZZ_GET_MODIFIED_FIELDS(
          exporting
            IS_SRC_HEAD  = LS_HEAD_BASE_DATA
            IS_DST_HEAD = LO_BO_HEAD->DATAREF->*
            IS_SRC_ITEM  = LS_ITEM_BASE_DATA
            IS_DST_ITEM = LO_BO_ITEM->DATAREF->*
          importing
            ET_MODIFIED_FIELDS = LT_MODIFIEDFIELDS
        ).

        " 1. round
        ZZ_DO_FIELDMAPPING(
          exporting
            IO_BO_HEAD = LO_BO_HEAD
            IO_BO_ITEM = LO_BO_ITEM
            IT_MODIFIEDFIELDS = LT_MODIFIEDFIELDS ).

        " 2. round
        LS_HEAD_BASE_DATA = LO_BO_HEAD->DATAREF->*.
        LS_ITEM_BASE_DATA = LO_BO_ITEM->DATAREF->*.

        LO_BO_ITEM->DATAREF->WDPLANT = GS_PROCESS_DATA-FACILITYID.
        LO_BO_ITEM->DATAREF->WASTE = GS_PROCESS_DATA-WASTEID.

        ZZ_GET_MODIFIED_FIELDS(
          exporting
            IS_SRC_HEAD  = LS_HEAD_BASE_DATA
            IS_DST_HEAD = LO_BO_HEAD->DATAREF->*
            IS_SRC_ITEM  = LS_ITEM_BASE_DATA
            IS_DST_ITEM = LO_BO_ITEM->DATAREF->*
          importing
            ET_MODIFIED_FIELDS = LT_MODIFIEDFIELDS
        ).

        ZZ_DO_FIELDMAPPING(
          IO_BO_HEAD = LO_BO_HEAD
          IO_BO_ITEM = LO_BO_ITEM
          IT_MODIFIEDFIELDS = LT_MODIFIEDFIELDS
        ).

        ZZ_DO_FILL_ARB_DATA(
          IO_BO_HEAD = LO_BO_HEAD
          IO_BO_ITEM = LO_BO_ITEM
        ).

        if GS_PROCESS_DATA-WEIGHINGID is initial.
          LO_BO_ITEM->BOOK_WEIGHING( PAR_DEVICEGROUP = GS_PROCESS_DATA-DEVICEGROUP ).
        else.
          LO_BO_ITEM->BOOK_WEIGHING_BACK( PAR_DEVICEGROUP = GS_PROCESS_DATA-DEVICEGROUP ).
        endif.

      cleanup.
        LO_BO_HEAD->UNLOCK_FOR_EDIT( ).
*          create object LEX type YCX_SERVICE exporting TEXTID = YCX_SERVICE=>WEIGHING_FAILED.
*          DO_ADD_MESSAGE( PAR_TYPE = 'E' PAR_TEXT = LEX->GET_TEXT( ) ).
    endtry.

    try.
        GS_RESULT_DATA-WEIGHINGID   = LO_BO_HEAD->DATAREF->WEIGHINGNR.
        GS_RESULT_DATA-BRUTTOWEIGHT = LO_BO_ITEM->DATAREF->BRUTTO_WEIGHT.
        GS_RESULT_DATA-TARAWEIGHT   = LO_BO_ITEM->DATAREF->TARA_WEIGHT.
        GS_RESULT_DATA-NETTOWEIGHT  = LO_BO_ITEM->DATAREF->NETTO_WEIGHT.
        GS_RESULT_DATA-WEIGHTUNIT   = LO_BO_ITEM->DATAREF->BRUTTO_UNIT.
        if IV_SIMULATION = ABAP_FALSE.
          LO_BO_HEAD->MODIFY_DATA( ).
        endif.
      cleanup.
        clear GS_RESULT_DATA.
        LO_BO_HEAD->UNLOCK_FOR_EDIT( ).
*          create object LEX type YCX_SERVICE exporting TEXTID = YCX_SERVICE=>WEIGHPROC_SAVE_FAILED.
*          DO_ADD_MESSAGE( PAR_TYPE = 'E' PAR_TEXT = LEX->GET_TEXT( ) ).
    endtry.

    if GS_PROCESS_DATA-WEIGHINGID is not initial. "return weighing
      " FINALIZING
      try.
          LO_BO_HEAD->BOOK_FINISH( ).
          LO_BO_HEAD->MODIFY_DATA( ).

        cleanup.
          LO_BO_HEAD->UNLOCK_FOR_EDIT( ).
*            create object LEX type YCX_SERVICE exporting TEXTID = YCX_SERVICE=>WEIGHPROC_FINISH_FAILED.
*            DO_ADD_MESSAGE( PAR_TYPE = 'W' PAR_TEXT = LEX->GET_TEXT( ) ).
*            LWARNING = ABAP_TRUE.
      endtry.

    endif.

*    catch CX_EEWA_BASE into LO_EX.
*      /WATP/CL_MISC=>ROLLBACK_WORK( ). "rollback work.
*      DO_ADD_MESSAGE( PAR_TYPE = 'E' PAR_TEXT = LEX_EEWA->GET_TEXT( ) ).
*  endtry.
    endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_WR_WAA_TA_SELFWEIGHING->ZZ_GET_MODIFIED_FIELDS
* +-------------------------------------------------------------------------------------------------+
* | [--->] IS_SRC_HEAD                    TYPE        EWASWA_MULTIWEIGHPROC_HEAD
* | [--->] IS_DST_HEAD                    TYPE        EWASWA_MULTIWEIGHPROC_HEAD
* | [--->] IS_SRC_ITEM                    TYPE        EWASWA_MULTIWEIGHPROC_ITEM
* | [--->] IS_DST_ITEM                    TYPE        EWASWA_MULTIWEIGHPROC_ITEM
* | [<---] ET_MODIFIED_FIELDS             TYPE        EWAPDDICFIELDNAMES
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_GET_MODIFIED_FIELDS.
    DATA:
          LT_CHANGED type /WATP/PCOPYFIELDNAMES,
          LY_CHANGED type ref to /WATP/SCOPYFIELDNAME.

    clear ET_MODIFIED_FIELDS.

    /WATP/CL_MISC=>COMPARE_DATA(
      exporting
        PAR_FROMDATA      = IS_SRC_HEAD
        PAR_TODATA        = IS_DST_HEAD
      importing
        PAR_CHANGEDFIELDS = LT_CHANGED
    ).

    loop at LT_CHANGED reference into LY_CHANGED.
      insert LY_CHANGED->NAME_TO into table ET_MODIFIED_FIELDS.
    endloop.

    clear LT_CHANGED.

    /WATP/CL_MISC=>COMPARE_DATA(
      exporting
        PAR_FROMDATA      = IS_SRC_ITEM
        PAR_TODATA        = IS_DST_ITEM
      importing
        PAR_CHANGEDFIELDS = LT_CHANGED
    ).

    loop at LT_CHANGED reference into LY_CHANGED.
      insert LY_CHANGED->NAME_TO into table ET_MODIFIED_FIELDS.
    endloop.

    sort ET_MODIFIED_FIELDS.
    delete adjacent duplicates from ET_MODIFIED_FIELDS.

  endmethod.
ENDCLASS.
