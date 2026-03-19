class ZCL_PLCP_TA_WA_ORDER_RSLT definition
  public
  inheriting from /PLCP/CL_TA_WA_ORDER_RSLT
  create public .

public section.

  class-methods CLASS_CONSTRUCTOR .

  methods BOOK_ATTACHMENTS
    redefinition .
  methods BOOK_ORDER_ITEMS
    redefinition .
protected section.

  class-data:
    GT_CONFNOTE_TA  TYPE STANDARD TABLE OF /PLCE/SPDCNFNT_KEY .
  class-data GV_RUN_ORIGINAL type CHAR1 .

  methods ZZ_IMPORT_INFO_CONFNOTES
    importing
      !IT_CONFNOTES type /PLCP/PWA_CONFNOTE_RSLT
      !IO_BO type ref to CL_EEWA_BO_BASE
    raising
      CX_EEWA_BASE .
  methods ZZ_UPDATE_ELOCSD_NEW
    importing
      !ID_SERV_RESULT type ref to /PLCP/SWA_WDOI_RSLT .
  methods BOOK_ORDER_ITEMS_SPEC
    importing
      !PAR_SEQUENCE_ONLY type KENNZX default SPACE
    raising
      CX_EEWA_BASE .
  methods ZZ_CONFIRM_WDOI
    importing
      !IO_BO_WDOI type ref to ZCL_WR_EEWA_BO_WDORDERITEM .
  methods ZZ_UPD_WDOI_FROM_SERVICE
    importing
      !IO_BO_WDOI type ref to ZCL_WR_EEWA_BO_WDORDERITEM .

  methods CALL_METHOD
    redefinition .
  methods EXECUTE_DATA_UPLOAD
    redefinition .
  methods MOVE_ITEM_DETAILS
    redefinition .
  methods PREPARE_RESULT_DATA_ITEM
    redefinition .
  methods PREPARE_RESULT_DATA_ITEM_ATTA
    redefinition .
  methods PREPARE_RESULT_DATA_ITEM_CN
    redefinition .
private section.
ENDCLASS.



CLASS ZCL_PLCP_TA_WA_ORDER_RSLT IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_PLCP_TA_WA_ORDER_RSLT->BOOK_ATTACHMENTS
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method BOOK_ATTACHMENTS.
    data:
      LS_DATA          type ZWR_GOS_CREATE_INFO,
      LO_FILENAME      type ref to CL_FS_PATH,
      LV_EXT           type STRING,
      LV_SUCCESS       type ABAP_BOOL,
      LO_BO            type ref to CL_EEWA_BO_BASE,
      LO_BO_ORDER      type ref to CL_EEWA_BO_WDORDER,
      LT_DBATTACHMENTS type standard table of /PLCP/TWRATTA,
      LV_METAGOSID     type /WATP/DOBJNR,
      LV_BOR_TYPE      type SWO_OBJTYP,
      LT_LOCAL_ATT_RESULT type standard table of /PLCP/SWA_ATTACHMENT_RSLT.

    LT_LOCAL_ATT_RESULT = ATTACHMENTRESULTS.
    clear ATTACHMENTRESULTS.

*    SUPER->BOOK_ATTACHMENTS( ).
    loop at LT_LOCAL_ATT_RESULT reference into data(LATTACHMENTREF) where DATA is not initial.
      clear LS_DATA.
      select single UUID from /PLCP/TWRATTA into @data(LDOCID_EXIST)
        where UUID = @LATTACHMENTREF->DATA->ATTACHMENTUUID.

      if SY-SUBRC is not initial.
        if LATTACHMENTREF->DATA->FILENAME is not initial.
          LO_FILENAME = CL_FS_PATH=>CREATE(
            exporting
              NAME = LATTACHMENTREF->DATA->FILENAME
              PATH_KIND = CL_FS_PATH=>PATH_KIND_SMART
          ).
          LS_DATA-FILE_DATA-FILENAME = LO_FILENAME->GET_FILE_NAME( ).
          LV_EXT = TO_UPPER( LO_FILENAME->GET_FILE_EXTENSION( ) ).
        else.
          LV_EXT = GET_DOC_TYPE( PAR_MIMETYPE = LATTACHMENTREF->DATA->MIMETYPE ).
        endif.
        shift LV_EXT left deleting leading '.'.
        LS_DATA-FILE_DATA-DOC_TYPE = LV_EXT.

        if LV_EXT is not initial and STRLEN( LV_EXT ) = 3.
          if LATTACHMENTREF->TOURUUID is not initial and LATTACHMENTREF->SERVICEUUID is initial.
            read table FORDER_KEYS
              with key
                /PLCP/PD_TOUR_ID = LATTACHMENTREF->/PLCP/PD_TOUR_ID
                ORDER_DATE = LATTACHMENTREF->ORDER_DATE
                ROUTE = LATTACHMENTREF->ROUTE reference into data(LKEYREF) binary search.

            if SY-SUBRC is initial and LKEYREF->ORDERNR is not initial.
              TA_GETBO_VAR LO_BO_ORDER CL_EEWA_BO_WDORDER=>COT_WDORDER LKEYREF->ORDERNR.
              LO_BO ?= LO_BO_ORDER.
            else.
              raise exception type /PLCP/CX_EEWA_BASE
                message id '/PLCP/MC_WA'
                type 'E'
                number 031 ##NUMBER_OK
                with LATTACHMENTREF->/PLCP/PD_TOUR_ID.
            endif.

          else.
            select single ORDERNR, ORDER_LAUFNR from EWA_ORDER_OBJECT where POBJNR = @LATTACHMENTREF->POBJNR into @data(LITEMKEY).
            TA_GETBO_VAR LO_BO_ORDER CL_EEWA_BO_WDORDER=>COT_WDORDER LITEMKEY-ORDERNR.
            TA_GETBO_VAR LO_BO       CL_EEWA_BO_WDORDERITEM=>COT_WDORDERITEM LITEMKEY.
          endif.

          LV_BOR_TYPE = LO_BO->GET_GOS_BOR_OBJTYPE( ).
          if ZCL_WR_MISC_GOS=>CL_GET_IS_METAGOS_ACTIVE( IV_BOR_TYPE = LV_BOR_TYPE ) = ABAP_FALSE.
            insert LATTACHMENTREF->* into table ATTACHMENTRESULTS.
            SUPER->BOOK_ATTACHMENTS( ).
            clear ATTACHMENTRESULTS.
          else.
            LS_DATA-BOR_OBJECT-TYPEID = LV_BOR_TYPE.
            LS_DATA-BOR_OBJECT-INSTID = LO_BO->GET_GOS_BOR_OBJKEY( ).
            LS_DATA-DESCRIPTION = LATTACHMENTREF->ZZ_COMMENTS.

            LS_DATA-SOURCE_DATA-ATTA_SOURCE_TSTAMP = LATTACHMENTREF->ZZ_TIMESTAMP.
            LS_DATA-SOURCE_DATA-ATTA_SOURCE_USER = LATTACHMENTREF->ZZ_CREATED_BY.
            LS_DATA-SOURCE_DATA-ATTA_SOURCE = LATTACHMENTREF->ZZ_ATTA_SOURCE.
            LS_DATA-SOURCE_DATA-ATTA_SOURCE_PERNR = LATTACHMENTREF->ZZ_ATTA_SOURCE_PERNR.
            LS_DATA-FILE_DATA-CONTENT = LATTACHMENTREF->DATA->ATTACHMENT.

            if LATTACHMENTREF->ZZ_ATTA_SOURCE_CONFNOTE is not initial.
              LS_DATA-FILE_DATA-FILENAME = |{ LATTACHMENTREF->ZZ_ATTA_SOURCE_CONFNOTE };{ LS_DATA-SOURCE_DATA-ATTA_SOURCE_TSTAMP }.{ LS_DATA-FILE_DATA-DOC_TYPE }|.
            endif.

            LS_DATA-TITLE = LS_DATA-FILE_DATA-FILENAME.
            if LS_DATA-TITLE is initial.
              LS_DATA-TITLE = 'EXT'.
            endif.

            call function 'Z_WR_CREATE_ATTACHMENT'
              exporting
                IS_DATA           = LS_DATA
              importing
                EV_SUCCEED        = LV_SUCCESS
                EV_METAGOSID      = LV_METAGOSID
              exceptions
                ATTACHMENT_FAILED = 1
                others            = 2.
            if SY-SUBRC <> 0.
              CL_EEWA_MISC=>RAISE_SYMSG( ).
            else.
              insert value #( UUID = LATTACHMENTREF->DATA->ATTACHMENTUUID DOC_ID = LV_METAGOSID ) into table LT_DBATTACHMENTS.
            endif.
          endif.

        else.
          if LV_EXT is initial.
            raise exception type /PLCP/CX_EEWA_BASE
              message id '/PLCP/MC_WA'
              type 'E'
              number 032.
          else.
            raise exception type /PLCP/CX_EEWA_BASE
              message id '/PLCP/MC_WA'
              type 'E'
              number 033 ##NUMBER_OK.
          endif.

        endif.
      endif.

    endloop.

    ATTACHMENTRESULTS = LT_LOCAL_ATT_RESULT.

    if LINES( LT_DBATTACHMENTS ) is not initial.
      insert /PLCP/TWRATTA from table LT_DBATTACHMENTS.
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZCL_PLCP_TA_WA_ORDER_RSLT->BOOK_ORDER_ITEMS
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_SEQUENCE_ONLY              TYPE        KENNZX (default =SPACE)
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method BOOK_ORDER_ITEMS.
  try.
    ZCL_WR_EEWA_BO_WDORDERITEM=>SV_DISABLE_STATUS_LOG = ABAP_TRUE.
    BOOK_ORDER_ITEMS_SPEC( PAR_SEQUENCE_ONLY = PAR_SEQUENCE_ONLY ).
    cleanup.
      ZCL_WR_EEWA_BO_WDORDERITEM=>SV_DISABLE_STATUS_LOG = ABAP_FALSE.
  endtry.
  ZCL_WR_EEWA_BO_WDORDERITEM=>SV_DISABLE_STATUS_LOG = ABAP_FALSE.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->BOOK_ORDER_ITEMS_SPEC
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_SEQUENCE_ONLY              TYPE        KENNZX (default =SPACE)
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD BOOK_ORDER_ITEMS_SPEC.
* &1 Detailname
* &2 Dataref
* &3 Where clause
  DEFINE BO_LOOP_DETAIL_REF_WHERE.
    IF LLPDPATH&1 IS NOT INITIAL.
      LOOP AT <LLPDITEMS&1> REFERENCE INTO LLPDREF&1 WHERE (&3).
        ASSIGN (LLPDPATH&1) TO <LLPDITEM&1>.
        GET REFERENCE OF <LLPDITEM&1> INTO &2.
  END-OF-DEFINITION.

  DATA:
    LITEMKEYS_ACT       TYPE STANDARD TABLE OF TITEMORDERKEY,
    LITEMKEYREF         TYPE REF TO TITEMORDERKEY,
    LORDERNRS           TYPE STANDARD TABLE OF EORDERNR,
    LBO                 TYPE REF TO cl_eewa_BO_WDORDER,
    LITEMREF            TYPE REF TO EWASWDORDERITEMDET,
    LSERVICECAT         TYPE SERVICE_CAT,
    LBO_ITEM            TYPE REF TO ZCL_WR_EEWA_BO_WDORDERITEM,
    LT_ITEM_KEY_CONFIRM TYPE STANDARD TABLE OF EWA_ORDER_OBJECT_IKEY.


  IF LINES( ORDERITEMRESULTS ) IS NOT INITIAL.

**** Get Current keys.
**** Determine Items to move
**** Move Items
**** Fill Result data

*** Move items
    PREPARE_ORDER_ITEMS( ).

*** D&D is done. Targets exists.
*      clear LITEMKEYS_ACT.

    SELECT * INTO CORRESPONDING FIELDS OF TABLE LITEMKEYS_ACT FROM EWA_ORDER_OBJECT
      INNER JOIN EWA_ORDER_HEAD ON EWA_ORDER_OBJECT~ORDERNR = EWA_ORDER_HEAD~ORDERNR
      FOR ALL ENTRIES IN ORDERITEMRESULTS
      WHERE POBJNR = ORDERITEMRESULTS-POBJNR
        AND EWA_ORDER_HEAD~ORDER_DATE = ORDERITEMRESULTS-ORDER_DATE
        AND /PLCP/PD_TOUR_ID = ORDERITEMRESULTS-/PLCP/PD_TOUR_ID
        AND ROUTE = ORDERITEMRESULTS-ROUTE.
**   todo Status

    LORDERNRS = VALUE #( FOR L IN LITEMKEYS_ACT ( L-ORDERNR ) ).

    SORT LORDERNRS.
    DELETE ADJACENT DUPLICATES FROM LORDERNRS.

    LOOP AT LORDERNRS REFERENCE INTO DATA(LORDERNRREF).
      TRY.

          IF LBO IS INITIAL OR LBO->DATAREF->ORDERNR <> LORDERNRREF->*.
            LBO ?= GET_ORDER_BO( PAR_KEY = LORDERNRREF->* PAR_LOCK = 'X' ).
            LBO->RELOAD_DATA( ).
          ENDIF.


          BO_DATA_LOOP_DETAIL_VAR LBO ITEM.

          TRY.
              LOOP AT LITEMKEYS_ACT REFERENCE INTO LITEMKEYREF WHERE ORDERNR = LBO->DATAREF->ORDERNR.

                BO_LOOP_DETAIL_REF_WHERE ITEM LITEMREF 'POBJNR = LITEMKEYREF->POBJNR'.
                LBO_ITEM ?= LBO->GETDETAILBO(
                  EXPORTING
                    PAR_NAME        = 'ITEM'
                    PAR_DETAILINDEX = LITEMREF->DETAIL_INDEX
                ).
                ZZ_UPD_WDOI_FROM_SERVICE( IO_BO_WDOI = LBO_ITEM ). "#36113 - WP-3.1.11 P2C2-26
                FWDOIDEFAULTS = VALUE #( BASE FWDOIDEFAULTS ( CORRESPONDING #( LITEMREF->* ) ) ).

                LOOP AT ORDERITEMRESULTS REFERENCE INTO DATA(LRESULTREF)
                   WHERE POBJNR = LITEMKEYREF->POBJNR
                     AND ( /PLCP/PD_SERVICE_UUID = LITEMREF->/PLCP/PD_SERVICE_UUID ).

                  ZZ_UPDATE_ELOCSD_NEW( ID_SERV_RESULT = LRESULTREF ).

                  IF PAR_SEQUENCE_ONLY IS INITIAL.
**** Move results
                    LSERVICECAT = CL_EEWA_BO_SERVICETYPE=>CL_GET_SERVICE_CAT( PAR_SERVICE_TYPE = LITEMREF->SERVICE_TYPE ).

                    CASE LSERVICECAT.
                      WHEN CL_EEWA_BO_SERVICETYPE=>CSC_TRANSPORT.

                        LITEMREF->BEHTYP_RM = LRESULTREF->BEH_TYPE_NEW.
                        LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL_NEW.
                        LITEMREF->/PLCP/TIDNR_RM = LRESULTREF->TIDNR_NEW.
                        IF LRESULTREF->TIDNR_NEW IS NOT INITIAL.
                          SELECT SINGLE SERNR FROM EQUI AS EQ INNER JOIN EQUZ AS EZ ON EZ~EQUNR = EQ~EQUNR
                              WHERE EQ~MATNR = @LITEMREF->BEHTYP_RM
                                AND ( EZ~DATBI <= @LITEMREF->ORDER_DATE AND EZ~DATAB >= @LITEMREF->ORDER_DATE )
                                INTO @LITEMREF->SERNR_RM_NEW. "REMSERNR
                        ENDIF.
** SERNR

                      WHEN CL_EEWA_BO_SERVICETYPE=>CSC_SWITCH.

                        LITEMREF->BEHTYP_RM = LRESULTREF->BEH_TYPE_NEW.
                        LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL_NEW.
                        LITEMREF->/PLCP/TIDNR_NEW_RM = LRESULTREF->TIDNR_NEW.
                        LITEMREF->/PLCP/TIDNR_RM = LRESULTREF->TIDNR.

                        IF LRESULTREF->TIDNR_NEW IS NOT INITIAL.
                          SELECT SINGLE SERNR FROM EQUI AS EQ INNER JOIN EQUZ AS EZ ON EZ~EQUNR = EQ~EQUNR
                              WHERE EQ~MATNR = @LITEMREF->BEHTYP_RM
                                AND ( EZ~DATBI <= @LITEMREF->ORDER_DATE AND EZ~DATAB >= @LITEMREF->ORDER_DATE )
                                INTO @LITEMREF->SERNR_RM_NEW.
                        ENDIF.

                      WHEN OTHERS.

                        LITEMREF->BEHTYP_RM = LRESULTREF->BEH_TYPE.
                        LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL.
                        LITEMREF->BEH_TYPE_NEW = LRESULTREF->BEH_TYPE_NEW.
                        IF LRESULTREF->BEH_ANZAHL_NEW IS NOT INITIAL.
                          LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL_NEW.
                        ENDIF.
                        IF LRESULTREF->TIDNR_NEW IS NOT INITIAL.
                          LITEMREF->/PLCP/TIDNR_NEW_RM = LRESULTREF->TIDNR_NEW.
                          SELECT SINGLE SERNR FROM EQUI AS EQ INNER JOIN EQUZ AS EZ ON EZ~EQUNR = EQ~EQUNR
                            WHERE EQ~MATNR = @LITEMREF->BEH_TYPE_NEW
                              AND ( EZ~DATBI <= @LITEMREF->ORDER_DATE AND EZ~DATAB >= @LITEMREF->ORDER_DATE )
                              INTO @LITEMREF->SERNR_RM_NEW.
                        ENDIF.
                        IF LRESULTREF->TIDNR IS NOT INITIAL.
                          LITEMREF->/PLCP/TIDNR_RM = LRESULTREF->TIDNR.
                        ENDIF.

                    ENDCASE.

*                    LITEMREF->WASTE = LRESULTREF->WASTE_TYPE.
                    LITEMREF->WASTE_TYPE = LRESULTREF->WASTE_TYPE.
                    LITEMREF->WASTE = LRESULTREF->WASTE_TYPE.
                    LITEMREF->WASTE_WEIGHT = LRESULTREF->WASTE_WEIGHT.
                    LITEMREF->WEIGHT_UNIT = LRESULTREF->WEIGHT_UNIT.

***** todo SERNR NEW based on TIDNR!

                    LOOP AT LRESULTREF->CONFNOTES REFERENCE INTO DATA(LCONFNOTEREF).
                      IF LITEMREF->CONFTYPE1 IS INITIAL.
                        LITEMREF->CONFTYPE1 = LCONFNOTEREF->CONFTYPE.
                        LITEMREF->TEXT = LITEMREF->TEXT && ` ` && LCONFNOTEREF->CONFNOTE_COMMENT.
                      ELSEIF LITEMREF->CONFTYPE2 IS INITIAL.
                        LITEMREF->CONFTYPE2 = LCONFNOTEREF->CONFTYPE.
                        LITEMREF->TEXT = LITEMREF->TEXT && ` ` && LCONFNOTEREF->CONFNOTE_COMMENT.
                      ELSEIF LITEMREF->CONFTYPE3 IS INITIAL.
                        LITEMREF->CONFTYPE3 = LCONFNOTEREF->CONFTYPE.
                        LITEMREF->TEXT = LITEMREF->TEXT && ` ` && LCONFNOTEREF->CONFNOTE_COMMENT.
                      ENDIF.
                    ENDLOOP.

                    ZZ_IMPORT_INFO_CONFNOTES(
                      IO_BO   = LBO_ITEM
                      IT_CONFNOTES = LRESULTREF->ZZ_CONFNOTES_INFO ).

                    CONVERT TIME STAMP LRESULTREF->TIMESTAMP TIME ZONE SY-ZONLO
                      INTO TIME LITEMREF->ACTUAL_TIME.

                  ENDIF.

**** Fields which has to be updated always
                  LITEMREF->/PLCP/PD_SERVICE_UUID = LRESULTREF->/PLCP/PD_SERVICE_UUID.
                  LITEMREF->ROUTE_SEQUENCE = LRESULTREF->ROUTE_SEQUENCE.

                  DO_ADDITIONAL_SERVICES(
                    EXPORTING
                      PAR_BO_ORDERHEAD = LBO
                      PAR_ITEMREF = LITEMREF
                      PAR_ADDITIONAL_SERVICES = LRESULTREF->ADDITIONAL_SERVICES
                    ).

                ENDLOOP.
                IF SY-SUBRC IS INITIAL.
                  LBO->ITEM_CHANGED( PAR_DETAILINDEX = LITEMREF->DETAIL_INDEX ).
                ENDIF.
**#36097 WP-3.1.11 P2C2-25 Auto.confirmation /*
                IF PAR_SEQUENCE_ONLY IS INITIAL.
                  INSERT LBO_ITEM->DATAREF->KEY INTO TABLE LT_ITEM_KEY_CONFIRM.
*                  ZZ_CONFIRM_WDOI( IO_BO_WDOI = LBO_ITEM ).
                ENDIF.
* */
                BO_ENDLOOP_DETAIL.

              ENDLOOP.

              TA_BO_SAVE LBO.

            CLEANUP.
              TA_BO_UNLOCK LBO.
          ENDTRY.

          TA_BO_UNLOCK LBO.

          SORT LT_ITEM_KEY_CONFIRM.
          DELETE ADJACENT DUPLICATES FROM LT_ITEM_KEY_CONFIRM.

          LOOP AT LT_ITEM_KEY_CONFIRM INTO DATA(LS_ITEM_KEY).
            LBO_ITEM ?= GETBO(
              EXPORTING
                PAR_OBJTYPE   = CL_EEWA_BO_WDORDERITEM=>COT_WDORDERITEM
                PAR_KEY       = LS_ITEM_KEY
                PAR_LOCK      = 'X'              " PAR_LOCK
            ).
            TRY.
                ZZ_CONFIRM_WDOI( IO_BO_WDOI = LBO_ITEM ).
                SAVE_BO( LBO_ITEM ).
              CLEANUP.
                LBO_ITEM->UNLOCK_FOR_EDIT( ).
            ENDTRY.
            LBO_ITEM->UNLOCK_FOR_EDIT( ).
          ENDLOOP.

        CATCH CX_EEWA_BASE INTO EXCEPTION.
          CALLBACK_REPORT(
            PAR_KIND      = EXCEPTION->MSGTYPE
            PAR_EXCEPTION = EXCEPTION
          ).
      ENDTRY.

    ENDLOOP.

    HANDLE_CHANGED_ITEM_DATA( ).

  ENDIF.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->CALL_METHOD
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_PARAMS                     TYPE        ABAP_PARMBIND_TAB(optional)
* | [--->] PAR_NAME                       TYPE        CSEQUENCE
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CALL_METHOD.
  TA_CALL_METHOD.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_PLCP_TA_WA_ORDER_RSLT=>CLASS_CONSTRUCTOR
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method CLASS_CONSTRUCTOR.
    DATA:
          LV_CONFRESET type /PLCE/CPDCNFNT-ZZ_CONF_RESET.

    LV_CONFRESET = 'X'.

    select CONFIRMATION_NOTE
      into table GT_CONFNOTE_TA
      from /PLCE/CPDCNFNT
      where ZZ_CONF_RESET = LV_CONFRESET.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->EXECUTE_DATA_UPLOAD
* +-------------------------------------------------------------------------------------------------+
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method EXECUTE_DATA_UPLOAD.
    data:
      LORDERNRS   type standard table of EORDERNR,
      LORDERDATAS type standard table of EWA_ORDER_HEAD,
      LEXPORTDATA type ISU_WA_ORDERDOWN_S_EROUTE,
      LLOGHANDLE  type BALLOGHNDL,
      LLOG        type ref to CL_EEWA_IMPL_BO_CALLBACK_MLOG.


    LORDERNRS = value #( for L in FCHANGED_DATAS ( L-ORDERNR ) ).

    if LINES( LORDERNRS ) is not initial.

      sort LORDERNRS.
      delete adjacent duplicates from LORDERNRS.
      sort FCHANGED_DATAS by KEY.

      DATA_BO_RANGE X.
      SET_BO_RANGE_KEYS X __KEYS LORDERNRS.

      CL_EEWA_OR_WDORDER=>CL_READ(
        exporting
          PAR_OBJTYPE = CL_EEWA_BO_WDORDER=>COT_WDORDER
          PAR_RANGES  = LRANGEXS
        importing
          PAR_TABLE   = LORDERDATAS
      ).

      if LINES( LORDERDATAS ) is not initial.

        loop at LORDERDATAS reference into data(LORDERDATARF).
          insert initial line into table LEXPORTDATA-T_EORDER reference into data(LORDEREXPORTREF).
          LORDEREXPORTREF->WA_EORDER = LORDERDATARF->*.
          loop at FCHANGED_DATAS reference into data(LITEMDATARF) where ORDERNR = LORDERDATARF->ORDERNR.
            insert initial line into table LORDEREXPORTREF->T_ORDER_OBJ reference into data(LITMEEXPORTREF).
            LITMEEXPORTREF->WA_ORDER_OBJ = corresponding #( LITEMDATARF->* ).
          endloop.
        endloop.

        if LEXPORTDATA is not initial.
****      Log handle
          if FCALLBACK is not initial and FCALLBACK is instance of CL_EEWA_IMPL_BO_CALLBACK_MLOG.
            LLOG ?= FCALLBACK.
            LLOGHANDLE = LLOG->MSGLOG->HANDLE.
          endif.

          new ZCL_WR_PD_WA_WDOI_PDSERVICE( LLOGHANDLE )->CREATE_BY_PRINTDATA(
            exporting
              IS_PRINT_DATA = LEXPORTDATA
          ).
        endif.
      endif.
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->MOVE_ITEM_DETAILS
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_SOURCE                     TYPE REF TO CL_EEWA_BO_WDORDERITEM
* | [--->] PAR_TARGET                     TYPE REF TO CL_EEWA_BO_WDORDERITEM
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method MOVE_ITEM_DETAILS.
    data:
          LO_BO_WDOI_TARGET type ref to ZCL_WR_EEWA_BO_WDORDERITEM,
          LO_BO_WDOI_SOURCE type ref to ZCL_WR_EEWA_BO_WDORDERITEM.

    ASSIGNCAST LO_BO_WDOI_TARGET PAR_TARGET.
    ASSIGNCAST LO_BO_WDOI_SOURCE PAR_SOURCE.

    if LO_BO_WDOI_SOURCE is not initial.
      "attention! this is not standard...see redefinition!
      LO_BO_WDOI_SOURCE->REQUIRE_DETAILS(
        PAR_DETAILS   = '*'
        PAR_RELATIONS = '*'
      ).
      "MJ: we cant use required here otherwise the same detail entries based on POBJNR-Key will be loaded
      "    this will cause double entries
      LO_BO_WDOI_TARGET->INIT_DATA(
        PAR_DETAILS   = '*'
        PAR_RELATIONS = '*'
      ).
      SUPER->MOVE_ITEM_DETAILS(
        PAR_SOURCE = PAR_SOURCE
        PAR_TARGET = PAR_TARGET
      ).
      if LO_BO_WDOI_TARGET is bound.
        LO_BO_WDOI_TARGET->ZZ_BOOK_MOVED_FROM( IO_SOURCE = PAR_SOURCE ).
      endif.
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->PREPARE_RESULT_DATA_ITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_DATAREF                    TYPE REF TO /PLCE/SWR_SERVICE_RSLT
* | [--->] PAR_HEADREF                    TYPE REF TO /PLCP/SWA_WDO_RSLT
* | [<---] PAR_ITEMREF                    TYPE REF TO /PLCP/SWA_WDOI_RSLT
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD PREPARE_RESULT_DATA_ITEM.

    SUPER->PREPARE_RESULT_DATA_ITEM(
      EXPORTING
        PAR_DATAREF = PAR_DATAREF
        PAR_HEADREF = PAR_HEADREF
      IMPORTING
        PAR_ITEMREF = PAR_ITEMREF
    ).

    PAR_ITEMREF->WASTE_WEIGHT = PAR_DATAREF->RESULT-AMOUNT.
    PAR_ITEMREF->WEIGHT_UNIT = PAR_DATAREF->RESULT-AMOUNTUNIT.

    select single SRV_RESULT~zzlongitude, SRV_RESULT~zzlatitude
      from /PLCE/R_PDMTourServiceResultCS as SRV_RESULT
      where ServiceUUID = @PAR_DATAREF->SERVICEUUID
      into (@PAR_ITEMREF->ZZLONGITUDE,@PAR_ITEMREF->ZZLATITUDE).
  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->PREPARE_RESULT_DATA_ITEM_ATTA
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_DATAREF                    TYPE REF TO /PLCE/PWR_SERVICE_ATTA_RSLT
* | [--->] PAR_HEADREF                    TYPE REF TO /PLCP/SWA_WDO_RSLT
* | [--->] PAR_ITEMREF                    TYPE REF TO /PLCP/SWA_WDOI_RSLT
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method PREPARE_RESULT_DATA_ITEM_ATTA.
    data:
      LS_SERV_ATT_DATA type /PLCE/TPDSRVATTM,
      LV_SOURCE_PERNR	 type ZWR_DGOS_ATTA_SOURCE_PERNR,
      LV_ID_NUMBER     type BU_ID_NUMBER.

    loop at PAR_DATAREF->* reference into data(LSRVATTACHMENTREF).
      clear:
        LS_SERV_ATT_DATA,
        LV_SOURCE_PERNR.

      insert initial line into table ATTACHMENTRESULTS reference into data(LSRVATTAREF).

      select single *
        into LS_SERV_ATT_DATA
        from /PLCE/TPDSRVATTM
        where ATTACHMENT_UUID = LSRVATTACHMENTREF->ATTACHMENTUUID.

      if SY-SUBRC is not initial. "Attachment list was filled up by RA-Attachments
        select single DOCS~*, CONFNOTE~CONFIRMATION_NOTE
          into @DATA(LS_DOC_ATTA)
          from /PLCE/TPDMDOCRES as DOCS
          left outer join /PLCE/TPDMCONFNT as CONFNOTE on CONFNOTE~UUID = DOCS~CONFIRMATION_UUID
          where DOCS~UUID = @LSRVATTACHMENTREF->ATTACHMENTUUID.

        if SY-SUBRC is initial.
          LSRVATTAREF->ZZ_COMMENTS = LS_DOC_ATTA-DOCS-COMMENTS.
          LSRVATTAREF->ZZ_CREATED_BY = LS_DOC_ATTA-DOCS-CREATED_BY.
          LSRVATTAREF->ZZ_TIMESTAMP = LS_DOC_ATTA-DOCS-TIMESTAMP.
          LSRVATTAREF->ZZ_ATTA_SOURCE = ZCL_WR_MISC_GOS=>GV_ATTA_SOURCE-SOURCE_RA.
          LSRVATTAREF->ZZ_ATTA_SOURCE_CONFNOTE = LS_DOC_ATTA-CONFIRMATION_NOTE.

          select single IDNUMBER
            into LV_ID_NUMBER
            from BUT0ID
              inner join /PLCE/TMDPLRSSRC as PD_RES on PD_RES~PARTNER = BUT0ID~PARTNER
              where PD_RES~RESOURCE_ID = LS_DOC_ATTA-DOCS-RESOURCE_ID and
                    BUT0ID~TYPE = ZCL_WR_ORD_MISC=>GV_IDTYPE_EMPLOYEE.
          if SY-SUBRC is initial.
            LV_SOURCE_PERNR = LV_ID_NUMBER.
          endif.
        endif.
      else.
        LSRVATTAREF->ZZ_COMMENTS = LS_SERV_ATT_DATA-COMMENTS.
        LSRVATTAREF->ZZ_CREATED_BY = LS_SERV_ATT_DATA-CREATED_BY.
        LSRVATTAREF->ZZ_TIMESTAMP = LS_SERV_ATT_DATA-CREATED_AT.
        LSRVATTAREF->ZZ_ATTA_SOURCE = ZCL_WR_MISC_GOS=>GV_ATTA_SOURCE-SOURCE_D.
      endif.

      LSRVATTAREF->/PLCP/SWA_WDOH_KEY = PAR_HEADREF->/PLCP/SWA_WDOH_KEY.
      LSRVATTAREF->POBJNR = PAR_ITEMREF->POBJNR.
      LSRVATTAREF->SERVICEUUID = LSRVATTACHMENTREF->SERVICEUUID.
      LSRVATTAREF->DATA = ref #( LSRVATTACHMENTREF->/PLCE/SWR_ATTACHMENT_DATA ).
    endloop.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->PREPARE_RESULT_DATA_ITEM_CN
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_DATAREF                    TYPE REF TO /PLCE/PWR_CONFNOTE_RSLT
* | [--->] PAR_HEADREF                    TYPE REF TO /PLCP/SWA_WDO_RSLT
* | [--->] PAR_ITEMREF                    TYPE REF TO /PLCP/SWA_WDOI_RSLT
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method PREPARE_RESULT_DATA_ITEM_CN.
    data:
*          LS_DETAIL_DATA type /PLCE/TPDMCONFNT,
      LV_SOURCE_PERNR	type ZWR_DGOS_ATTA_SOURCE_PERNR,
      LV_ID_NUMBER    type BU_ID_NUMBER.

    loop at PAR_DATAREF->* reference into data(LSRVCONFNOTEREF).
      clear:
       LV_SOURCE_PERNR.

      select single CONF~*, CONF_CONF~ZZ_CONF_RESET
        into @data(LS_DETAIL_DATA)
        from /PLCE/TPDMCONFNT as CONF
          inner join /PLCE/CPDCNFNT as CONF_CONF on CONF_CONF~CONFIRMATION_NOTE = CONF~CONFIRMATION_NOTE
        where TOUR_UUID = @LSRVCONFNOTEREF->TOURUUID and
              SERVICE_UUID = @LSRVCONFNOTEREF->SERVICEUUID and
              TIMESTAMP = @LSRVCONFNOTEREF->TIMESTAMP.
      if SY-SUBRC is initial.
        select single IDNUMBER
           into LV_ID_NUMBER
           from BUT0ID
             inner join /PLCE/TMDPLRSSRC as PD_RES on PD_RES~PARTNER = BUT0ID~PARTNER
             where PD_RES~RESOURCE_ID = LS_DETAIL_DATA-CONF-RESOURCE_ID and
                   BUT0ID~TYPE = ZCL_WR_ORD_MISC=>GV_IDTYPE_EMPLOYEE.
        if SY-SUBRC is initial.
          LV_SOURCE_PERNR = LV_ID_NUMBER.
        endif.
      endif.

      if LS_DETAIL_DATA-ZZ_CONF_RESET = ABAP_FALSE.
        if LSRVCONFNOTEREF->EXTERNALID is not initial.
          insert initial line into table PAR_ITEMREF->CONFNOTES reference into data(LITEMCONFNOTEREF).
          LITEMCONFNOTEREF->CONFTYPE = LSRVCONFNOTEREF->EXTERNALID.
          LITEMCONFNOTEREF->CONFNOTE_COMMENT = LS_DETAIL_DATA-CONF-COMMENTS.
        else. "Log
          PAR_HEADREF->INVALID_RESULT = 'X'.
        endif.
      elseif LSRVCONFNOTEREF->/PLCE/SWR_ATTACHMENT_DATA-ATTACHMENT is initial.
        "all confirmation notes of type INFO
        if LSRVCONFNOTEREF->EXTERNALID is not initial.
          insert initial line into table PAR_ITEMREF->ZZ_CONFNOTES_INFO reference into data(LS_INFO_CONFNOTES).
          LS_INFO_CONFNOTES->CONFTYPE = LSRVCONFNOTEREF->EXTERNALID.
          LS_INFO_CONFNOTES->CONFNOTE_COMMENT        = LS_DETAIL_DATA-CONF-COMMENTS.
          LS_INFO_CONFNOTES->ZZ_CREATED_BY           = LS_DETAIL_DATA-CONF-CREATED_BY.
          LS_INFO_CONFNOTES->ZZ_TIMESTAMP            = LSRVCONFNOTEREF->TIMESTAMP.
          LS_INFO_CONFNOTES->ZZ_ATTA_SOURCE          = ZCL_WR_MISC_GOS=>GV_ATTA_SOURCE-SOURCE_RA.
          LS_INFO_CONFNOTES->ZZ_ATTA_SOURCE_PERNR    = LV_SOURCE_PERNR.
          LS_INFO_CONFNOTES->ZZ_ATTA_SOURCE_CONFNOTE = LSRVCONFNOTEREF->CONFIRMATIONNOTE.
        endif.
      endif.

      "direct attached attachment to confirmation note
      if LSRVCONFNOTEREF->/PLCE/SWR_ATTACHMENT_DATA-ATTACHMENT is not initial.
        insert initial line into table ATTACHMENTRESULTS reference into data(LCONFSRVATTAREF).
        LCONFSRVATTAREF->/PLCP/SWA_WDOH_KEY = PAR_ITEMREF->/PLCP/SWA_WDOH_KEY.
        LCONFSRVATTAREF->TOURUUID = LSRVCONFNOTEREF->TOURUUID.
        LCONFSRVATTAREF->SERVICEUUID = LSRVCONFNOTEREF->SERVICEUUID.
        LCONFSRVATTAREF->POBJNR = PAR_ITEMREF->POBJNR.
        if LSRVCONFNOTEREF->/PLCE/SWR_ATTACHMENT_DATA is not initial.
          LCONFSRVATTAREF->DATA = ref #( LSRVCONFNOTEREF->/PLCE/SWR_ATTACHMENT_DATA ).
        endif.
        LCONFSRVATTAREF->ZZ_COMMENTS = LS_DETAIL_DATA-CONF-COMMENTS.
        LCONFSRVATTAREF->ZZ_CREATED_BY = LS_DETAIL_DATA-CONF-CREATED_BY.
        LCONFSRVATTAREF->ZZ_TIMESTAMP = LSRVCONFNOTEREF->TIMESTAMP.
        LCONFSRVATTAREF->ZZ_ATTA_SOURCE = ZCL_WR_MISC_GOS=>GV_ATTA_SOURCE-SOURCE_RA.
        LCONFSRVATTAREF->ZZ_ATTA_SOURCE_PERNR = LV_SOURCE_PERNR.
        LCONFSRVATTAREF->ZZ_ATTA_SOURCE_CONFNOTE = LSRVCONFNOTEREF->CONFIRMATIONNOTE.
      endif.
    endloop.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->ZZ_CONFIRM_WDOI
* +-------------------------------------------------------------------------------------------------+
* | [--->] IO_BO_WDOI                     TYPE REF TO ZCL_WR_EEWA_BO_WDORDERITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD zz_confirm_wdoi.
*#36097 WP-3.1.11 P2C2-25 DEV Automatic confirmation in EWAWDOC
  DATA:
    ltourid      TYPE /plce/pdtour_id,
    lservice     TYPE /plce/swr_service_rslt,
    lresultdatas TYPE /plce/pwr_tour_rslt ##NEEDED.

  CHECK io_bo_wdoi->dataref->/plcp/pd_service_uuid IS NOT INITIAL.

  IF line_exists( orderitemresults[ pobjnr = io_bo_wdoi->dataref->pobjnr ] ).
    ltourid = orderitemresults[ pobjnr = io_bo_wdoi->dataref->pobjnr ]-/plcp/pd_tour_id.
  ENDIF.

  IF ltourid IS NOT INITIAL.
    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_true.
    TRY.
        io_bo_wdoi->check_book_confirm_pos( ).
        io_bo_wdoi->book_confirm_pos( ).
      CATCH cx_eewa_base INTO DATA(lex).
        "Wird IMMER ausgeführt – auch bei Exception
        zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_false.
        RAISE EXCEPTION lex.
    ENDTRY.
     "WICHTIG: auch im Erfolgsfall zurücksetzen
    zcl_wr_eewa_bo_wdorderitem=>zzv_auto_confirm = abap_false.
  ENDIF.
ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->ZZ_IMPORT_INFO_CONFNOTES
* +-------------------------------------------------------------------------------------------------+
* | [--->] IT_CONFNOTES                   TYPE        /PLCP/PWA_CONFNOTE_RSLT
* | [--->] IO_BO                          TYPE REF TO CL_EEWA_BO_BASE
* | [!CX!] CX_EEWA_BASE
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_IMPORT_INFO_CONFNOTES.
    DATA:
          LS_DATA type ZWR_GOS_CREATE_INFO.

    loop at IT_CONFNOTES reference into DATA(LD_CONFNOTE).
      LS_DATA-BOR_OBJECT-TYPEID = IO_BO->GET_BOR_OBJTYPE( ).
      LS_DATA-BOR_OBJECT-INSTID = IO_BO->GET_GOS_BOR_OBJKEY( ).
      LS_DATA-DESCRIPTION = LD_CONFNOTE->CONFNOTE_COMMENT.

      LS_DATA-SOURCE_DATA-ATTA_SOURCE_TSTAMP = LD_CONFNOTE->ZZ_TIMESTAMP.
      LS_DATA-SOURCE_DATA-ATTA_SOURCE_USER = LD_CONFNOTE->ZZ_CREATED_BY.
      LS_DATA-SOURCE_DATA-ATTA_SOURCE = LD_CONFNOTE->ZZ_ATTA_SOURCE.
      LS_DATA-SOURCE_DATA-ATTA_SOURCE_PERNR = LD_CONFNOTE->ZZ_ATTA_SOURCE_PERNR.

      LS_DATA-FILE_DATA-FILENAME = |{ LD_CONFNOTE->ZZ_ATTA_SOURCE_CONFNOTE };{ LD_CONFNOTE->ZZ_TIMESTAMP }|.

      LS_DATA-TITLE = LS_DATA-FILE_DATA-FILENAME.
      if LS_DATA-TITLE is initial.
        LS_DATA-TITLE = 'EXT_NOTE'.
      endif.

      call function 'Z_WR_CREATE_NOTE'
        exporting
          IS_DATA                 = LS_DATA
       EXCEPTIONS
         ATTACHMENT_FAILED       = 1
         OTHERS                  = 2.
      if SY-SUBRC <> 0.
        CL_EEWA_MISC=>RAISE_SYMSG( ).
      endif.
    endloop.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->ZZ_UPDATE_ELOCSD_NEW
* +-------------------------------------------------------------------------------------------------+
* | [--->] ID_SERV_RESULT                 TYPE REF TO /PLCP/SWA_WDOI_RSLT
* +--------------------------------------------------------------------------------------</SIGNATURE>
  method ZZ_UPDATE_ELOCSD_NEW.
    data:
      LT_CONTASSIGN     type ISUWA_EWAELOCSDNEW,
      LT_CONTASSIGN_NEW type ISUWA_EWAELOCSDNEW,
      LV_UPD_MODE	      type DAMODUS value 'U'.


    select CONTASSGN~*
        into table @LT_CONTASSIGN
        from EWA_ORDER_OBJECT as EAP
        inner join EWAOBJ as SERV on SERV~OBJNR = EAP~SERVFREQNR
        inner join EWAELOCSD_NEW as CONTASSGN on CONTASSGN~VBELN = EAP~SDAUFNR and
                                                 CONTASSGN~POSNR = EAP~SDPOSNR and
                                                 CONTASSGN~EQUNR = SERV~EQUNR and
                                                 CONTASSGN~IBASE = SERV~IBASE
        where POBJNR = @ID_SERV_RESULT->POBJNR and
              CONTASSGN~AB   <= EAP~ORDER_DATE and
              CONTASSGN~BIS  >= EAP~ORDER_DATE.

    if sy-subrc is initial and ID_SERV_RESULT->ZZLATITUDE is not initial.
      LT_CONTASSIGN_NEW = LT_CONTASSIGN.
      read table LT_CONTASSIGN_NEW reference into data(LR_CONTASSIGN_NEW) index 1.
      LR_CONTASSIGN_NEW->ZZLONGITUDE = ID_SERV_RESULT->ZZLONGITUDE.
      LR_CONTASSIGN_NEW->ZZLATITUDE = ID_SERV_RESULT->ZZLATITUDE.

      CL_EEWA_RCI_ELOC=>DB_EWAELOCSDNEW_UPDATE(
        exporting
          IT_EWAELOCSDNEW   = LT_CONTASSIGN_NEW
          IT_O_EWAELOCSDNEW = LT_CONTASSIGN
        changing
          C_UPD_MODE        = LV_UPD_MODE
        exceptions
          UPDATE_ERROR      = 1
          GENERAL_FAULT     = 2
          others            = 4
      ).
    endif.
  endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Protected Method ZCL_PLCP_TA_WA_ORDER_RSLT->ZZ_UPD_WDOI_FROM_SERVICE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IO_BO_WDOI                     TYPE REF TO ZCL_WR_EEWA_BO_WDORDERITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method ZZ_UPD_WDOI_FROM_SERVICE.
*#36113 WP-3.1.11: P2C2-26 DEV Change of service Types in the process
  data:
    LTOURID      type /PLCE/PDTOUR_ID,
    LSERVICE     type /PLCE/SWR_SERVICE_RSLT,
    LRESULTDATAS type /PLCE/PWR_TOUR_RSLT,
    LV_ACTION    type /PLCE/PDACTION,
    LV_SERVICE_TYPE type SERVICE_TYPE ##NEEDED.

  check IO_BO_WDOI->DATAREF->/PLCP/PD_SERVICE_UUID is not initial.

  if LINE_EXISTS( ORDERITEMRESULTS[ POBJNR = IO_BO_WDOI->DATAREF->POBJNR ] ).
    LTOURID = ORDERITEMRESULTS[ POBJNR = IO_BO_WDOI->DATAREF->POBJNR ]-/PLCP/PD_TOUR_ID.
  endif.

  if LTOURID is not initial and
     LINE_EXISTS( RESULTDATAS[ TOURID = LTOURID ] ) and
     LINE_EXISTS( RESULTDATAS[ TOURID = LTOURID ]-SERVICES[ SERVICEUUID = IO_BO_WDOI->DATAREF->/PLCP/PD_SERVICE_UUID ] ).

    LSERVICE = RESULTDATAS[ TOURID = LTOURID ]-SERVICES[ SERVICEUUID = IO_BO_WDOI->DATAREF->/PLCP/PD_SERVICE_UUID ].

    LV_SERVICE_TYPE = LSERVICE-ACTION.
  else.
    select single ACTION "#EC CI_SEL_NESTED
      from /PLCE/TPDSRV
      where SERVICE_UUID = @IO_BO_WDOI->DATAREF->/PLCP/PD_SERVICE_UUID
      into @data(LACTION).
    if SY-SUBRC is initial.
      LV_SERVICE_TYPE = LACTION.
    endif.
  endif.

  if LV_SERVICE_TYPE is not initial and IO_BO_WDOI->DATAREF->SERVICE_TYPE <> LV_SERVICE_TYPE.
    try.
        IO_BO_WDOI->ZZ_BOOK_CHANGE_STYPE( IV_SERVICE_TYPE = LV_SERVICE_TYPE ).
      catch CX_EEWA_BASE.
    endtry.
  endif.
endmethod.
ENDCLASS.
