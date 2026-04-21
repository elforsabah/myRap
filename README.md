class ZCL_WR_EEWA_WDOC_TREE_HELPER definition
  public
  create public .

public section.

  constants C_ITEMNAME_EXTRAICON type TV_ITMNAME value '4' ##NO_TEXT.
  constants C_ITEMNAME_EXTRAICON2 type TV_ITMNAME value '23' ##NO_TEXT.
  constants C_ITEMNAME_EXTRAICON3 type TV_ITMNAME value '24' ##NO_TEXT.
  constants C_ITEMNAME_EXTRAICON4 type TV_ITMNAME value '25' ##NO_TEXT.
  constants C_NODETYPE_ITEM type CHAR30 value 'ITEM' ##NO_TEXT.
  constants C_NODETYPE_ORDER type CHAR30 value 'ORDER' ##NO_TEXT.
  constants C_NODETYPE_WEIGH type CHAR30 value 'WEIGH' ##NO_TEXT.
  constants C_NODETYPE_WPROC type CHAR30 value 'WPROC' ##NO_TEXT.

  class-methods CLASS_CONSTRUCTOR .
  class-methods CL_ADJUST_NODES_ITEM
    importing
      !PAR_NODES type ISU_WAEL_TREEV_BASE_NODES
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
      !PAR_TREENODES type TREEV_NTAB .
  class-methods CL_ADJUST_NODES_ORDER
    importing
      !PAR_NODES type ISU_WAEL_TREEV_BASE_NODES
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
      !PAR_TREENODES type TREEV_NTAB .
  class-methods CL_ADJUST_NODES_WEIGH
    importing
      !PAR_NODES type ISU_WAEL_TREEV_BASE_NODES
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
      !PAR_TREENODES type TREEV_NTAB .
  class-methods CL_GET_DESCITEM
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
  class-methods CL_GET_EXTRAICONITEM
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
  class-methods CL_GET_EXTRAICONITEM2
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
  class-methods CL_GET_EXTRAICONITEM3
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
  class-methods CL_GET_EXTRAICONITEM4
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
  class-methods CL_GET_LABELITEM
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
  class-methods CL_GET_PROPVALUE
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
      !PAR_NAME type STRING
    changing
      value(PAR_VALUE) type ANY .
  class-methods CL_GET_STATEITEM
    importing
      !PAR_NODE type ref to EWAEL_TREEV_NODE_BASE
    changing
      !PAR_ITEMS type ISU_WAEL_TREEV_ITEMS
    returning
      value(PAR_REF) type ref to EWAEL_TREEV_ITEM .
protected section.
private section.
ENDCLASS.



CLASS ZCL_WR_EEWA_WDOC_TREE_HELPER IMPLEMENTATION.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CLASS_CONSTRUCTOR
* +-------------------------------------------------------------------------------------------------+
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CLASS_CONSTRUCTOR.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_ADJUST_NODES_ITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODES                      TYPE        ISU_WAEL_TREEV_BASE_NODES
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-->] PAR_TREENODES                  TYPE        TREEV_NTAB
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_ADJUST_NODES_ITEM.
  data:
    LR_TREENODE type ref to TREEV_NODE ##NEEDED,
    LR_NODE     type ref to EWAEL_TREEV_NODE_BASE ##NEEDED.

  data:
    LV_OBJNR  type J_OBJNR,
    LT_OBJNRS type BO_T_OBJNR.

  loop at PAR_NODES reference into LR_NODE where TYPE = C_NODETYPE_ITEM. "#EC CI_SORTSEQ
    CL_GET_PROPVALUE(
      exporting
        PAR_NODE  = LR_NODE
        PAR_NAME  = 'POBJNR'
      changing
        PAR_VALUE = LV_OBJNR ) ##NO_TEXT.
    insert LV_OBJNR into table LT_OBJNRS.
  endloop.

  if LT_OBJNRS is initial.
    return.
  endif.

  types:
    begin of TS_OI_INFO,
      POBJNR         type EWA_ORDER_OBJECT-POBJNR,
      ZZ_POBJNR_MAIN type EWA_ORDER_OBJECT-ZZ_POBJNR_MAIN,
      ZZ_POBJNR_COMM type EWA_ORDER_OBJECT-ZZ_POBJNR_COMM,
    end of TS_OI_INFO.
  types:
    TT_OI_INFO type standard table of TS_OI_INFO with key POBJNR.
  data(LT_OI_INFOS) = value TT_OI_INFO( ).
  select *
    from EWA_ORDER_OBJECT as A
    for all entries in @LT_OBJNRS
  where A~POBJNR = @LT_OBJNRS-TABLE_LINE
   into corresponding fields of table @LT_OI_INFOS.

  loop at PAR_NODES reference into LR_NODE where TYPE = C_NODETYPE_ITEM. "#EC CI_SORTSEQ
    data(LR_DESCREF) = CL_GET_DESCITEM( exporting PAR_NODE = LR_NODE changing PAR_ITEMS = PAR_ITEMS ).
    data(LR_LABELREF) = CL_GET_LABELITEM( exporting PAR_NODE = LR_NODE changing PAR_ITEMS = PAR_ITEMS ).
    if LR_DESCREF is not bound or LR_LABELREF is not bound.
      continue.
    endif.
    CL_GET_PROPVALUE(
      exporting
        PAR_NODE  = LR_NODE
        PAR_NAME  = 'POBJNR'
      changing
        PAR_VALUE = LV_OBJNR ) ##NO_TEXT.
    if LV_OBJNR is initial.
      continue.
    endif.
    try.
        data(LS_OI_INFO) = LT_OI_INFOS[ POBJNR = LV_OBJNR ].
        if LS_OI_INFO-ZZ_POBJNR_MAIN is not initial.
          if LS_OI_INFO-ZZ_POBJNR_MAIN = LS_OI_INFO-POBJNR.
            LR_DESCREF->TEXT = ZCL_WR_EEWA_WDOC_MISC=>CL_CONVERT_OBJNR_OUT( PAR_POBJNR = LS_OI_INFO-ZZ_POBJNR_MAIN ) && ` (I) ` && LR_DESCREF->TEXT ##NO_TEXT.
          else.
            LR_DESCREF->TEXT = `_` && ZCL_WR_EEWA_WDOC_MISC=>CL_CONVERT_OBJNR_OUT( PAR_POBJNR = LS_OI_INFO-ZZ_POBJNR_MAIN ) && ` ` && LR_DESCREF->TEXT ##NO_TEXT.
          endif.
        endif.
      catch CX_SY_ITAB_LINE_NOT_FOUND ##NO_HANDLER.
    endtry.
*    read table LORDERITEMS reference into LORDERITEM with key POBJNR = LPOBJNR.
*    if SY-SUBRC is initial.

*    call function 'CONVERSION_EXIT_EWAPO_OUTPUT'
*      exporting
*        INPUT         = LV_OBJNR
*      importing
*        OUTPUT        = LR_LABELREF->TEXT
*      exceptions
*        CONVERT_ERROR = 1
*        others        = 2 ##FM_SUBRC_OK.
    " COL
*    if LORDERITEM->YY_POBJNR_MAIN is not initial and LORDERITEM->YY_POBJNR_MAIN <> LORDERITEM->POBJNR.
    "  LR_LABELREF->TEXT = LLABELREF->TEXT && 'Test' .
*    |_{ LLABELREF->TEXT }|. " Unterstrich vor Label = COL-Unterposition
*    endif.
*    LR_LABELREF->TEXT = LR_LABELREF->TEXT && ` (` && LV_OBJNR && `)` ##NO_TEXT.
*    LR_LABELREF->STYLE = COL_GROUP. "Gelb
    "LR_DESCREF->TEXT = LR_DESCREF->TEXT && 'Test'.
    "Lr_DESCREF->STYLE = COL_GROUP.
    "CL_EEWA_MISC=>ICON_CREATE(
    "  exporting
    "    PAR_ICON   = ICON_MOVE
    "    PAR_INFO   = text-T02
    "  changing
    "    PAR_RESULT = Lr_DESCREF->T_IMAGE
    ").
  endloop.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_ADJUST_NODES_ORDER
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODES                      TYPE        ISU_WAEL_TREEV_BASE_NODES
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-->] PAR_TREENODES                  TYPE        TREEV_NTAB
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_ADJUST_NODES_ORDER.
*  data:
*    LR_TREENODE type ref to TREEV_NODE ##NEEDED,
*    LR_NODE     type ref to EWAEL_TREEV_NODE_BASE ##NEEDED.
*
*  data:
*    LV_OBJNR  type J_OBJNR,
*    LT_OBJNRS type BO_T_OBJNR.
*
*  loop at PAR_NODES reference into LR_NODE where TYPE = C_NODETYPE_ORDER. "#EC CI_SORTSEQ
*    CL_GET_PROPVALUE(
*      exporting
*        PAR_NODE = LR_NODE
*        PAR_NAME = 'HOBJNR'
*      changing PAR_VALUE = LV_OBJNR ) ##NO_TEXT.
*    insert LV_OBJNR into table LT_OBJNRS.
*  endloop.
*
*  if LT_OBJNRS is initial.
*    return.
*  endif.
*
*  loop at PAR_NODES reference into LR_NODE where TYPE = C_NODETYPE_ORDER. "#EC CI_SORTSEQ
*    data(LR_DESCREF) = CL_GET_DESCITEM( exporting PAR_NODE = LR_NODE changing PAR_ITEMS = PAR_ITEMS ).
*    data(LR_LABELREF) = CL_GET_LABELITEM( exporting PAR_NODE = LR_NODE changing PAR_ITEMS = PAR_ITEMS ).
*    if LR_DESCREF is not bound or LR_LABELREF is not bound.
*      continue.
*    endif.
*    CL_GET_PROPVALUE(
*      exporting
*        PAR_NODE = LR_NODE
*        PAR_NAME = 'HOBJNR'
*      changing PAR_VALUE = LV_OBJNR ) ##NO_TEXT.
**    LR_LABELREF->TEXT = LR_LABELREF->TEXT && ` (` && LV_OBJNR && `)` ##NO_TEXT.
**    LR_LABELREF->STYLE = COL_GROUP. "Gelb
*    "LR_DESCREF->TEXT = LDESCREF->TEXT && 'Test'.
*  endloop.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_ADJUST_NODES_WEIGH
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODES                      TYPE        ISU_WAEL_TREEV_BASE_NODES
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-->] PAR_TREENODES                  TYPE        TREEV_NTAB
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_ADJUST_NODES_WEIGH.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_DESCITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_DESCITEM.
  data:
    LITEM     type EWAEL_TREEV_ITEM,
    LITEMNAME type TV_ITMNAME.

  LITEMNAME = CL_EEWA_FORM_TREE=>C_ITEMNAME_LAST_HITEM + 2.

  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = LITEMNAME.
  if SY-SUBRC is not initial.
    LITEM-NODE_KEY  = PAR_NODE->NODE_KEY.
    LITEM-ITEM_NAME = LITEMNAME.
    LITEM-ALIGNMENT = CL_GUI_LIST_TREE=>ALIGN_AUTO.
    LITEM-CLASS     = CL_GUI_LIST_TREE=>ITEM_CLASS_TEXT.
    LITEM-EDITABLE  = SPACE.
    LITEM-FONT      = CL_GUI_LIST_TREE=>ITEM_FONT_PROP.
    insert LITEM into table PAR_ITEMS reference into PAR_REF.
  endif.

endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_EXTRAICONITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_EXTRAICONITEM.
  data:
    LITEM     type EWAEL_TREEV_ITEM.

  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = C_ITEMNAME_EXTRAICON.
  if SY-SUBRC is not initial.
    LITEM-NODE_KEY  = PAR_NODE->NODE_KEY.
    LITEM-ITEM_NAME = C_ITEMNAME_EXTRAICON.
    LITEM-ALIGNMENT = CL_GUI_LIST_TREE=>ALIGN_AUTO.
    LITEM-CLASS     = CL_GUI_LIST_TREE=>ITEM_CLASS_TEXT.
    LITEM-EDITABLE  = SPACE.
    LITEM-FONT      = CL_GUI_LIST_TREE=>ITEM_FONT_PROP.
    insert LITEM into table PAR_ITEMS reference into PAR_REF.
  endif.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_EXTRAICONITEM2
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_EXTRAICONITEM2.
  data:
    LITEM     type EWAEL_TREEV_ITEM.

  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = C_ITEMNAME_EXTRAICON2.
  if SY-SUBRC is not initial.
    LITEM-NODE_KEY  = PAR_NODE->NODE_KEY.
    LITEM-ITEM_NAME = C_ITEMNAME_EXTRAICON2.
    LITEM-ALIGNMENT = CL_GUI_LIST_TREE=>ALIGN_AUTO.
    LITEM-CLASS     = CL_GUI_LIST_TREE=>ITEM_CLASS_TEXT.
    LITEM-EDITABLE  = SPACE.
    LITEM-FONT      = CL_GUI_LIST_TREE=>ITEM_FONT_PROP.
    insert LITEM into table PAR_ITEMS reference into PAR_REF.
  endif.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_EXTRAICONITEM3
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_EXTRAICONITEM3.
  data:
    LITEM     type EWAEL_TREEV_ITEM.

  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = C_ITEMNAME_EXTRAICON3.
  if SY-SUBRC is not initial.
    LITEM-NODE_KEY  = PAR_NODE->NODE_KEY.
    LITEM-ITEM_NAME = C_ITEMNAME_EXTRAICON3.
    LITEM-ALIGNMENT = CL_GUI_LIST_TREE=>ALIGN_AUTO.
    LITEM-CLASS     = CL_GUI_LIST_TREE=>ITEM_CLASS_TEXT.
    LITEM-EDITABLE  = SPACE.
    LITEM-FONT      = CL_GUI_LIST_TREE=>ITEM_FONT_PROP.
    insert LITEM into table PAR_ITEMS reference into PAR_REF.
  endif.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_EXTRAICONITEM4
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_EXTRAICONITEM4.
  data:
    LITEM     type EWAEL_TREEV_ITEM.

  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = C_ITEMNAME_EXTRAICON4.
  if SY-SUBRC is not initial.
    LITEM-NODE_KEY  = PAR_NODE->NODE_KEY.
    LITEM-ITEM_NAME = C_ITEMNAME_EXTRAICON4.
    LITEM-ALIGNMENT = CL_GUI_LIST_TREE=>ALIGN_AUTO.
    LITEM-CLASS     = CL_GUI_LIST_TREE=>ITEM_CLASS_TEXT.
    LITEM-EDITABLE  = SPACE.
    LITEM-FONT      = CL_GUI_LIST_TREE=>ITEM_FONT_PROP.
    insert LITEM into table PAR_ITEMS reference into PAR_REF.
  endif.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_LABELITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_LABELITEM.
  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = CL_EEWA_FORM_TREE=>C_ITEMNAME_LABEL.
  if SY-SUBRC is not initial.
    clear PAR_REF.
  endif.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_PROPVALUE
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [--->] PAR_NAME                       TYPE        STRING
* | [<-->] PAR_VALUE                      TYPE        ANY
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_PROPVALUE.
  data:
      LFIELD type ref to EWAEL_PROPVALUE.

  read table PAR_NODE->FIELDS reference into LFIELD
    with key NAME = PAR_NAME ##WARN_OK.
  if SY-SUBRC is initial.
    PAR_VALUE = LFIELD->VALUE.
  endif.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Static Public Method ZCL_WR_EEWA_WDOC_TREE_HELPER=>CL_GET_STATEITEM
* +-------------------------------------------------------------------------------------------------+
* | [--->] PAR_NODE                       TYPE REF TO EWAEL_TREEV_NODE_BASE
* | [<-->] PAR_ITEMS                      TYPE        ISU_WAEL_TREEV_ITEMS
* | [<-()] PAR_REF                        TYPE REF TO EWAEL_TREEV_ITEM
* +--------------------------------------------------------------------------------------</SIGNATURE>
method CL_GET_STATEITEM.
  read table PAR_ITEMS reference into PAR_REF
    with key NODE_KEY  = PAR_NODE->NODE_KEY
             ITEM_NAME = CL_EEWA_FORM_TREE=>C_ITEMNAME_STATE.
  if SY-SUBRC is not initial.
    clear PAR_REF.
  endif.

endmethod.
ENDCLASS.
