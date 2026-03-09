method BOOK_ORDER_ITEMS_SPEC.
* &1 Detailname
* &2 Dataref
* &3 Where clause
  define BO_LOOP_DETAIL_REF_WHERE.
    if LLPDPATH&1 is not initial.
      loop at <LLPDITEMS&1> reference into LLPDREF&1 where (&3).
        assign (LLPDPATH&1) to <LLPDITEM&1>.
        get reference of <LLPDITEM&1> into &2.
  end-of-definition.

  data:
    LITEMKEYS_ACT       type standard table of TITEMORDERKEY,
    LITEMKEYREF         type ref to TITEMORDERKEY,
    LORDERNRS           type standard table of EORDERNR,
    LBO                 type ref to cl_eewa_BO_WDORDER,
    LITEMREF            type ref to EWASWDORDERITEMDET,
    LSERVICECAT         type SERVICE_CAT,
    LBO_ITEM            type ref to ZCL_WR_EEWA_BO_WDORDERITEM,
    LT_ITEM_KEY_CONFIRM type standard table of EWA_ORDER_OBJECT_IKEY.


  if LINES( ORDERITEMRESULTS ) is not initial.

**** Get Current keys.
**** Determine Items to move
**** Move Items
**** Fill Result data

*** Move items
    PREPARE_ORDER_ITEMS( ).

*** D&D is done. Targets exists.
*      clear LITEMKEYS_ACT.

    select * into corresponding fields of table LITEMKEYS_ACT from EWA_ORDER_OBJECT
      inner join EWA_ORDER_HEAD on EWA_ORDER_OBJECT~ORDERNR = EWA_ORDER_HEAD~ORDERNR
      for all entries in ORDERITEMRESULTS
      where POBJNR = ORDERITEMRESULTS-POBJNR
        and EWA_ORDER_HEAD~ORDER_DATE = ORDERITEMRESULTS-ORDER_DATE
        and /PLCP/PD_TOUR_ID = ORDERITEMRESULTS-/PLCP/PD_TOUR_ID
        and ROUTE = ORDERITEMRESULTS-ROUTE.
**   todo Status

    LORDERNRS = value #( for L in LITEMKEYS_ACT ( L-ORDERNR ) ).

    sort LORDERNRS.
    delete adjacent duplicates from LORDERNRS.

    loop at LORDERNRS reference into data(LORDERNRREF).
      try.

          if LBO is initial or LBO->DATAREF->ORDERNR <> LORDERNRREF->*.
            LBO ?= GET_ORDER_BO( PAR_KEY = LORDERNRREF->* PAR_LOCK = 'X' ).
            LBO->RELOAD_DATA( ).
          endif.


          BO_DATA_LOOP_DETAIL_VAR LBO ITEM.

          try.
              loop at LITEMKEYS_ACT reference into LITEMKEYREF where ORDERNR = LBO->DATAREF->ORDERNR.

                BO_LOOP_DETAIL_REF_WHERE ITEM LITEMREF 'POBJNR = LITEMKEYREF->POBJNR'.
                LBO_ITEM ?= LBO->GETDETAILBO(
                  exporting
                    PAR_NAME        = 'ITEM'
                    PAR_DETAILINDEX = LITEMREF->DETAIL_INDEX
                ).
                ZZ_UPD_WDOI_FROM_SERVICE( IO_BO_WDOI = LBO_ITEM ). "#36113 - WP-3.1.11 P2C2-26
                FWDOIDEFAULTS = value #( base FWDOIDEFAULTS ( corresponding #( LITEMREF->* ) ) ).

                loop at ORDERITEMRESULTS reference into data(LRESULTREF) where POBJNR = LITEMKEYREF->POBJNR.
                  ZZ_UPDATE_ELOCSD_NEW( ID_SERV_RESULT = LRESULTREF ).

                  if PAR_SEQUENCE_ONLY is initial.
**** Move results
                    LSERVICECAT = CL_EEWA_BO_SERVICETYPE=>CL_GET_SERVICE_CAT( PAR_SERVICE_TYPE = LITEMREF->SERVICE_TYPE ).

                    case LSERVICECAT.
                      when CL_EEWA_BO_SERVICETYPE=>CSC_TRANSPORT.

                        LITEMREF->BEHTYP_RM = LRESULTREF->BEH_TYPE_NEW.
                        LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL_NEW.
                        LITEMREF->/PLCP/TIDNR_RM = LRESULTREF->TIDNR_NEW.
                        if LRESULTREF->TIDNR_NEW is not initial.
                          select single SERNR from EQUI as EQ inner join EQUZ as EZ on EZ~EQUNR = EQ~EQUNR
                              where EQ~MATNR = @LITEMREF->BEHTYP_RM
                                and ( EZ~DATBI <= @LITEMREF->ORDER_DATE and EZ~DATAB >= @LITEMREF->ORDER_DATE )
                                into @LITEMREF->SERNR_RM_NEW. "REMSERNR
                        endif.
** SERNR

                      when CL_EEWA_BO_SERVICETYPE=>CSC_SWITCH.

                        LITEMREF->BEHTYP_RM = LRESULTREF->BEH_TYPE_NEW.
                        LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL_NEW.
                        LITEMREF->/PLCP/TIDNR_NEW_RM = LRESULTREF->TIDNR_NEW.
                        LITEMREF->/PLCP/TIDNR_RM = LRESULTREF->TIDNR.

                        if LRESULTREF->TIDNR_NEW is not initial.
                          select single SERNR from EQUI as EQ inner join EQUZ as EZ on EZ~EQUNR = EQ~EQUNR
                              where EQ~MATNR = @LITEMREF->BEHTYP_RM
                                and ( EZ~DATBI <= @LITEMREF->ORDER_DATE and EZ~DATAB >= @LITEMREF->ORDER_DATE )
                                into @LITEMREF->SERNR_RM_NEW.
                        endif.

                      when others.

                        LITEMREF->BEHTYP_RM = LRESULTREF->BEH_TYPE.
                        LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL.
                        LITEMREF->BEH_TYPE_NEW = LRESULTREF->BEH_TYPE_NEW.
                        if LRESULTREF->BEH_ANZAHL_NEW is not initial.
                          LITEMREF->BEH_ANZAHL_RM = LRESULTREF->BEH_ANZAHL_NEW.
                        endif.
                        if LRESULTREF->TIDNR_NEW is not initial.
                          LITEMREF->/PLCP/TIDNR_NEW_RM = LRESULTREF->TIDNR_NEW.
                          select single SERNR from EQUI as EQ inner join EQUZ as EZ on EZ~EQUNR = EQ~EQUNR
                            where EQ~MATNR = @LITEMREF->BEH_TYPE_NEW
                              and ( EZ~DATBI <= @LITEMREF->ORDER_DATE and EZ~DATAB >= @LITEMREF->ORDER_DATE )
                              into @LITEMREF->SERNR_RM_NEW.
                        endif.
                        if LRESULTREF->TIDNR is not initial.
                          LITEMREF->/PLCP/TIDNR_RM = LRESULTREF->TIDNR.
                        endif.

                    endcase.

*                    LITEMREF->WASTE = LRESULTREF->WASTE_TYPE.
                    LITEMREF->WASTE_TYPE = LRESULTREF->WASTE_TYPE.
                    LITEMREF->WASTE = LRESULTREF->WASTE_TYPE.
                    LITEMREF->WASTE_WEIGHT = LRESULTREF->WASTE_WEIGHT.
                    LITEMREF->WEIGHT_UNIT = LRESULTREF->WEIGHT_UNIT.

***** todo SERNR NEW based on TIDNR!

                    loop at LRESULTREF->CONFNOTES reference into data(LCONFNOTEREF).
                      if LITEMREF->CONFTYPE1 is initial.
                        LITEMREF->CONFTYPE1 = LCONFNOTEREF->CONFTYPE.
                        LITEMREF->TEXT = LITEMREF->TEXT && ` ` && LCONFNOTEREF->CONFNOTE_COMMENT.
                      elseif LITEMREF->CONFTYPE2 is initial.
                        LITEMREF->CONFTYPE2 = LCONFNOTEREF->CONFTYPE.
                        LITEMREF->TEXT = LITEMREF->TEXT && ` ` && LCONFNOTEREF->CONFNOTE_COMMENT.
                      elseif LITEMREF->CONFTYPE3 is initial.
                        LITEMREF->CONFTYPE3 = LCONFNOTEREF->CONFTYPE.
                        LITEMREF->TEXT = LITEMREF->TEXT && ` ` && LCONFNOTEREF->CONFNOTE_COMMENT.
                      endif.
                    endloop.

                    ZZ_IMPORT_INFO_CONFNOTES(
                      IO_BO   = LBO_ITEM
                      IT_CONFNOTES = LRESULTREF->ZZ_CONFNOTES_INFO ).

                    convert time stamp LRESULTREF->TIMESTAMP time zone SY-ZONLO
                      into time LITEMREF->ACTUAL_TIME.

                  endif.

**** Fields which has to be updated always
                  LITEMREF->/PLCP/PD_SERVICE_UUID = LRESULTREF->/PLCP/PD_SERVICE_UUID.
                  LITEMREF->ROUTE_SEQUENCE = LRESULTREF->ROUTE_SEQUENCE.

                  DO_ADDITIONAL_SERVICES(
                    exporting
                      PAR_BO_ORDERHEAD = LBO
                      PAR_ITEMREF = LITEMREF
                      PAR_ADDITIONAL_SERVICES = LRESULTREF->ADDITIONAL_SERVICES
                    ).

                endloop.
                if SY-SUBRC is initial.
                  LBO->ITEM_CHANGED( PAR_DETAILINDEX = LITEMREF->DETAIL_INDEX ).
                endif.
**#36097 WP-3.1.11 P2C2-25 Auto.confirmation /*
                if PAR_SEQUENCE_ONLY is initial.
                  insert LBO_ITEM->DATAREF->KEY into table LT_ITEM_KEY_CONFIRM.
*                  ZZ_CONFIRM_WDOI( IO_BO_WDOI = LBO_ITEM ).
                endif.
* */
                BO_ENDLOOP_DETAIL.

              endloop.

              TA_BO_SAVE LBO.

            cleanup.
              TA_BO_UNLOCK LBO.
          endtry.

          TA_BO_UNLOCK LBO.

          sort LT_ITEM_KEY_CONFIRM.
          delete adjacent duplicates from LT_ITEM_KEY_CONFIRM.

          loop at LT_ITEM_KEY_CONFIRM into data(LS_ITEM_KEY).
            LBO_ITEM ?= GETBO(
              exporting
                PAR_OBJTYPE   = CL_EEWA_BO_WDORDERITEM=>COT_WDORDERITEM
                PAR_KEY       = LS_ITEM_KEY
                PAR_LOCK      = 'X'              " PAR_LOCK
            ).
            try.
                ZZ_CONFIRM_WDOI( IO_BO_WDOI = LBO_ITEM ).
                SAVE_BO( LBO_ITEM ).
              cleanup.
                LBO_ITEM->UNLOCK_FOR_EDIT( ).
            endtry.
            LBO_ITEM->UNLOCK_FOR_EDIT( ).
          endloop.

        catch CX_EEWA_BASE into EXCEPTION.
          CALLBACK_REPORT(
            PAR_KIND      = EXCEPTION->MSGTYPE
            PAR_EXCEPTION = EXCEPTION
          ).
      endtry.

    endloop.

    HANDLE_CHANGED_ITEM_DATA( ).

  endif.

endmethod.
