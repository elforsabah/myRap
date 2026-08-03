class /WATP/CL_BO_CSCABNAVV definition
  public
  inheriting from /WATP/CL_BO_CSCABNAVV_BASE
  create public .

*"* public components of class /WATP/CL_BO_CSCABNAVV
*"* do not include other source files here!!!
public section.

  constants C_CS_CABNAVV_JOIN_UP type /WATP/DIMPL value 'CS_CABNAVV_JOIN_UP' ##NO_TEXT.
  constants COT_CS_CABAVV type /WATP/DOBJTYPE value 'CS_CABNAVV' ##NO_TEXT.

  methods AVV_JOIN_UP
    raising
      /WATP/CX_BASE .
  methods CONSTRUCTOR .
  methods CHECK_CABNAVV
    importing
      !PAR_CABNAVVNAME type ATNAM
      !PAR_MATNR type MATNR optional
    exporting
      !PAR_OK type CHAR1 .
  methods CHECK_CABNAVV_NAME
    importing
      !PAR_CABNAVVNAME type ATNAM .
  methods BOOK_AVV_FILL_UP
    raising
      /WATP/CX_BASE .

  methods COPY_FROM_BO
    redefinition .
  methods SET_SVOBJ
    redefinition .
protected section.
*"* protected components of class /WATP/CL_BO_CSCABNAVV
*"* do not include other source files here!!!

  methods AVV_FILL_UP
    importing
      !PAR_CABNAVVS type /WATP/PCSBO_CABAVVD
    raising
      /WATP/CX_BASE .
  methods CHECK_CABNAVV_NAME_INT
    importing
      !PAR_CABNAVVNAME type ATNAM .
  methods CHECK_CABNAVV_INT
    importing
      !PAR_CABNAVVNAME type ATNAM
      !PAR_MATNR type MATNR optional
    exporting
      !PAR_OK type CHAR1
      !PAR_CABNAVVS type /WATP/PCSBO_CABAVVD .

  methods AVV_CHECK_SPEC
    redefinition .
  methods DATA_LOADED
    redefinition .
  methods DATA_NEWED
    redefinition .
  methods INSERT_DATA_SPEC
    redefinition .
  methods LOAD_DATA_SPEC
    redefinition .
  methods PREPARE_INSERT
    redefinition .
  methods SAVE_DATA_SPEC
    redefinition .
private section.
*"* private components of class /WATP/CL_BO_CSCABNAVV
*"* do not include other source files here!!!
ENDCLASS.



CLASS /WATP/CL_BO_CSCABNAVV IMPLEMENTATION.


method AVV_CHECK_SPEC.
  data:
    LTEXT   type STRING,
    LARB_NW type /WATP/SARBNWINFO.

  SUPER->AVV_CHECK_SPEC( exporting PAR_AVV_ITEM = PAR_AVV_ITEM ).
  if IS_CHECK_DISABLED( ) is initial.
    if PAR_AVV_ITEM is not initial.
      if PAR_AVV_ITEM->ACTIVE is not initial.
* Fehlen noch Daten
        if PAR_AVV_ITEM->WDPLANT is initial.
          LTEXT = PAR_AVV_ITEM->AVVCODE.
          RAISE_BO_CHECK_DETAIL /WATP/CX_BO_CS CABNAVV_NO_WDPLANT AVV LTEXT.
        elseif PAR_AVV_ITEM->WASTEMODE is initial.
          LTEXT = PAR_AVV_ITEM->AVVCODE.
          RAISE_BO_CHECK_DETAIL /WATP/CX_BO_CS CABNAVV_NO_WASTEMODE AVV LTEXT.
        endif.
        /WATP/CL_BO_HW_WDPLANT=>CHECK_AVV_DELIVERY_STATIC(
          exporting
            PAR_WDPLANTNR = PAR_AVV_ITEM->WDPLANT                 " Identifikation der Entsorgungsanlage
            PAR_AVVCODE   = PAR_AVV_ITEM->AVVCODE                 " AVV-Code
        ).
      endif.
* Überprüfen der Daten aus dem Entsorgungnachweis
      if PAR_AVV_ITEM->NWNR is not initial
        or PAR_AVV_ITEM->NWINTNR is not initial.

        DATA_BO_RANGE X.
        if PAR_AVV_ITEM->NWNR is not initial.
          SET_BO_RANGE X NWNR PAR_AVV_ITEM->NWNR /WATP/DARBNWNR.
        endif.
        if PAR_AVV_ITEM->NWINTNR is not initial.
          SET_BO_RANGE X NWINTNR PAR_AVV_ITEM->NWINTNR /WATP/DARBNWINTNR.
        endif.

        LARB_NW = /WATP/CL_BO_ARBNW=>READ_ARBNW(
            exporting
              PAR_RANGES = LRANGEXS
*              PAR_NWNR        =  PAR_AVV_ITEM->NWNR                " Entsorgungsnachweisnummer
*              PAR_NWINTNR     =  PAR_AVV_ITEM->NWINTNR             " interne Nachweisnummer
        ).
        if LARB_NW is initial.
          if PAR_AVV_ITEM->NWNR is not initial. "fachlicher Schlüssel
            LTEXT = PAR_AVV_ITEM->NWNR.
          endif.
          if PAR_AVV_ITEM->NWINTNR is not initial. "technischer Schlüssel
            concatenate LTEXT '[' PAR_AVV_ITEM->NWINTNR ']' into LTEXT separated by SPACE.
          endif.
          RAISE_BO_CHECK_DETAIL /WATP/CX_BO_CS CABNAVV_INVALID_KEY AVV LTEXT.
        endif.
        if PAR_AVV_ITEM->AVVCODE <> LARB_NW-AVVCODE.
          LTEXT = PAR_AVV_ITEM->AVVCODE.
          RAISE_BO_CHECK_DETAIL /WATP/CX_BO_CS CABNAVV_INVALID_NW AVV LTEXT.
        endif.
        if PAR_AVV_ITEM->WDPLANT <> LARB_NW-WDPLANT.
          LTEXT = PAR_AVV_ITEM->AVVCODE.
          RAISE_BO_CHECK_DETAIL /WATP/CX_BO_CS CABNAVV_INVALID_WDPLANT AVV LTEXT.
        endif.
        if PAR_AVV_ITEM->WASTEMODE <> LARB_NW-WASTEMODE.
          LTEXT = PAR_AVV_ITEM->AVVCODE.
          RAISE_BO_CHECK_DETAIL /WATP/CX_BO_CS CABNAVV_INVALID_WASTEMODE AVV LTEXT.
        endif.
      endif.
    endif.
  endif.
endmethod.


  method AVV_FILL_UP.
**** FW: Introducing calling AVV_FIND due to the fact that this method is called in DATA_NEWED.
****     This leads to duplicated entries in case of a copy of the wastelist.
    data:
      IT_AVVMARA      type standard table of /WATP/TAVVMARA,
      RT_AVVMARA      type ref to            /WATP/TAVVMARA,
      LITEM           type ref to            /WATP/SCSBO_CABAVVD,
      LCABNAVV        type ref to            /WATP/SCSBO_CABAVVD,
      LTAVA           type standard table of /WATP/TARB_AVA,
      LRAVA           type ref to            /WATP/TARB_AVA,
      LTAVKM          type standard table of /WATP/TARB_AVKM,
      LRAVKM          type ref to            /WATP/TARB_AVKM,
      LINDEX          type                   INT4,
      LEXISTING_INDEX type I.

    loop at PAR_CABNAVVS reference into LCABNAVV.
      clear IT_AVVMARA.
      select *
        into corresponding fields of table IT_AVVMARA
        from /WATP/TAVVMARA                             "#EC CI_NOFIRST
       inner join /WATP/TAVVCATH
          on /WATP/TAVVCATH~AVVCODE = /WATP/TAVVMARA~AVVCODE
         and /WATP/TAVVCATH~LFDNR = /WATP/TAVVMARA~LFDNR
       where /WATP/TAVVMARA~MATNR       = LCABNAVV->MATNR
         and /WATP/TAVVCATH~VALID_FROM <= SY-DATUM
         and /WATP/TAVVCATH~VALID_TO   >= SY-DATUM.
      loop at IT_AVVMARA reference into RT_AVVMARA.
        clear:
          LTAVA,
          LTAVKM.
*     Ö-Norm-Katalog auslesen
        select *
          into table LTAVKM
          from /WATP/TARB_AVKM
          where MATNR = RT_AVVMARA->MATNR.
        if LTAVKM is initial.
          select *
            into table LTAVA
            from /WATP/TARB_AVA                         "#EC CI_NOFIRST
            where AVVCODE = RT_AVVMARA->AVVCODE.
          if LTAVA is initial.
            clear LEXISTING_INDEX.
            AVV_FIND(
              exporting
                PAR_AVVCODE         = RT_AVVMARA->AVVCODE                 " AVV-Code
                PAR_MATNR           = RT_AVVMARA->MATNR                   " Materialnummer
              importing
                PAR_DETAILINDEX     = LEXISTING_INDEX
            ).
            if LEXISTING_INDEX is initial.
              AVV_INSERT(
                importing
                  PAR_INDEX    = LINDEX
                  PAR_AVV_ITEM = LITEM
              ).
              LITEM->CABN_AVVWDPLANT = DATAREF->CABN_AVVWDPLANT.
              LITEM->AVVCODE         = RT_AVVMARA->AVVCODE.
              LITEM->MATNR           = RT_AVVMARA->MATNR.
              AVV_CHANGED( exporting PAR_DETAILINDEX = LITEM->DETAIL_INDEX ).
            endif.
          else.
            loop at LTAVA reference into LRAVA.
              clear LEXISTING_INDEX.
              AVV_FIND(
                exporting
                  PAR_AVVCODE         = RT_AVVMARA->AVVCODE                 " AVV-Code
                  PAR_MATNR           = RT_AVVMARA->MATNR                   " Materialnummer
                  PAR_AVKCODE         = LRAVA->AVKCODE
                  PAR_SPEZIFIKATION   = LRAVA->SPEZIFIKATION
                importing
                  PAR_DETAILINDEX     = LEXISTING_INDEX
              ).
              if LEXISTING_INDEX is initial.
                AVV_INSERT(
                  importing
                    PAR_INDEX    = LINDEX
                    PAR_AVV_ITEM = LITEM
                ).
                LITEM->CABN_AVVWDPLANT = DATAREF->CABN_AVVWDPLANT.
                LITEM->AVVCODE         = RT_AVVMARA->AVVCODE.
                LITEM->MATNR           = RT_AVVMARA->MATNR.
                LITEM->AVKCODE         = LRAVA->AVKCODE.
                LITEM->SPEZIFIKATION   = LRAVA->SPEZIFIKATION.
                AVV_CHANGED( exporting PAR_DETAILINDEX = LITEM->DETAIL_INDEX ).
              endif.
            endloop.
          endif.
        else.
          loop at LTAVKM reference into LRAVKM.
            clear LEXISTING_INDEX.
            AVV_FIND(
              exporting
                PAR_AVVCODE         = RT_AVVMARA->AVVCODE                 " AVV-Code
                PAR_MATNR           = RT_AVVMARA->MATNR                   " Materialnummer
                PAR_AVKCODE         = LRAVA->AVKCODE
                PAR_SPEZIFIKATION   = LRAVA->SPEZIFIKATION
              importing
                PAR_DETAILINDEX     = LEXISTING_INDEX
            ).
            if LEXISTING_INDEX is initial.
              AVV_INSERT(
                importing
                  PAR_INDEX    = LINDEX
                  PAR_AVV_ITEM = LITEM
              ).
              LITEM->CABN_AVVWDPLANT = DATAREF->CABN_AVVWDPLANT.
              LITEM->AVVCODE         = RT_AVVMARA->AVVCODE.
              LITEM->MATNR           = RT_AVVMARA->MATNR.
              LITEM->AVKCODE         = LRAVKM->AVKCODE.
              LITEM->SPEZIFIKATION   = LRAVKM->SPEZIFIKATION.
              AVV_CHANGED( exporting PAR_DETAILINDEX = LITEM->DETAIL_INDEX ).
            endif.
          endloop.
        endif.
      endloop.
    endloop.
  endmethod.


  method AVV_JOIN_UP.
*    Methode dient zum Abgleichen, der Merkmalswerte mit dem Detail,
*    wird Standardmäßig aber leer aus geliefert. Sollte es beim Kunden gewünscht sein,
*    den Schalter CS_CABNAVV_JOIN_UP in Objektconfig einschalten

    data:
      LDBCABNAVV   type ref to /WATP/SCSBO_CABAVVD,
      LDBCABNAVVS  type /WATP/PCSBO_CABAVVD,
      LDETAILTABLE type ref to DATA,
      LTAB         type standard table of MATNR
                     with non-unique sorted key SKEY components TABLE_LINE.

    field-symbols:
      <LTAB>       type any table,
      <LLINE>      type DATA,
      <LMATNR>     type MATNR.

    if /WATP/CL_BO_IMPLEMENTATION=>GETISACTIVE( PAR_IMPL = C_CS_CABNAVV_JOIN_UP ) is not initial.
      if DATAREF->MATNR is not initial
        and DATAREF->CABN_NAME is not initial.
        CHECK_CABNAVV_INT(
          exporting
            PAR_CABNAVVNAME = DATAREF->CABN_NAME
            PAR_MATNR = DATAREF->MATNR
          importing
            PAR_CABNAVVS = LDBCABNAVVS
        ).
        LDETAILTABLE = GETDETAILTABLE( PAR_NAME = 'AVV' ).
        assign LDETAILTABLE->* to <LTAB>.
        loop at <LTAB> assigning <LLINE>.
          assign component 'MATNR' of structure <LLINE> to <LMATNR>.
          if SY-SUBRC is initial.
            insert <LMATNR> into table LTAB.
          endif.
        endloop.
        loop at LDBCABNAVVS reference into LDBCABNAVV.
          read table LTAB with table key SKEY components TABLE_LINE = LDBCABNAVV->MATNR transporting no fields.
          if SY-SUBRC is initial.
            delete LDBCABNAVVS.
            continue.
          endif.
        endloop.
        if LINES( LDBCABNAVVS ) > 0.
          AVV_FILL_UP( PAR_CABNAVVS = LDBCABNAVVS ).
        endif.
      endif.
    endif.
  endmethod.


method BOOK_AVV_FILL_UP .
  data:
    LCABNAVVS  type /WATP/PCSBO_CABAVVD.

  BO_BOOK_START BOOK_AVV_FILL_UP.
* Detaildatensätze laut Merkmal erzeugen:
    CHECK_CABNAVV_INT(
      exporting
        PAR_CABNAVVNAME = DATAREF->CABN_NAME
        PAR_MATNR       = DATAREF->MATNR
      importing
*       PAR_OK = LOK
        PAR_CABNAVVS    = LCABNAVVS
    ).

    AVV_FILL_UP( PAR_CABNAVVS = LCABNAVVS ).

  BO_BOOK_END BOOK_AVV_FILL_UP.
endmethod.


method CHECK_CABNAVV .
* Merkmalswerte einlesen (und auf Korrektheit prüfen)
  CHECK_CABNAVV_INT(
    exporting
      PAR_CABNAVVNAME = PAR_CABNAVVNAME
      PAR_MATNR = PAR_MATNR
    importing
      PAR_OK = PAR_OK
  ).
endmethod.


method CHECK_CABNAVV_INT .
  data:
    LCABNTAB  type standard table of CABN,
    LCAWNTAB  type standard table of CAWN,
    LRCAWNTAB type ref to CAWN,
    LCABAVVD  type /WATP/TCSCABAVVD,
*   LAVVCATINFO type /WATP/SAVVCATINFO,
    LINFO     type /WATP/SMARAINFO,
    LMATLIST  type /WATP/PCS_MATNR,
    LMATNR    type MATNR.

  clear PAR_OK.
  clear PAR_CABNAVVS.

  if PAR_CABNAVVNAME is not initial.
*   Lesen des Merkmals
    call function 'CLSE_SELECT_CABN_VIA_NAME'
      exporting
        CHARACTERISTIC = PAR_CABNAVVNAME
      tables
        T_CABN         = LCABNTAB
      exceptions
        NO_ENTRY_FOUND = 1.
    if SY-SUBRC is initial.
*     Lesen der möglichen Werte für die Abfälle
      call function 'CLSE_SELECT_CAWN'
        tables
          IN_CABN        = LCABNTAB
          T_CAWN         = LCAWNTAB
        exceptions
          NO_ENTRY_FOUND = 1.
      if SY-SUBRC is initial.
*       Liste der möglichen Abfälle füllen
        if LCAWNTAB is not initial.
          PAR_OK = 'X'.
          DETERMINE_GLOBALS(
              PAR_CABNAVVNAME = PAR_CABNAVVNAME
              PAR_MATNR       = PAR_MATNR
          ).
          DETERMINE_MATERIALS( ).

          if LINES( FMATLIST ) > 0.
          select MATNR
            into table LMATLIST
            from MARA
            for all entries in FMATLIST
            where MATNR = FMATLIST-TABLE_LINE.
          endif.

          sort LMATLIST.

          if LINES( LMATLIST ) <> LINES( FMATLIST ).
            sort FMATLIST.
            loop at FMATLIST into LMATNR.
              read table LMATLIST with key TABLE_LINE = LMATNR transporting no fields.
              if SY-SUBRC is initial.
                LCABAVVD-MATNR = LMATNR.
                append LCABAVVD to PAR_CABNAVVS.
              else.
                clear PAR_CABNAVVS.
                clear PAR_OK.
*             Fehler: Das Abfallmaterial X für Merkmal PAR_CABNAVVNAME existiert nicht.
                message I009(/WATP/NCONTSERVICE) with PAR_CABNAVVNAME LMATNR.
                exit.
              endif.
            endloop.
          else.
            loop at FMATLIST into LMATNR.
              LCABAVVD-MATNR = LMATNR.
              append LCABAVVD to PAR_CABNAVVS.
            endloop.
          endif.
        else.
          message I008(/WATP/NCONTSERVICE) with PAR_CABNAVVNAME.
        endif.
      else.
*       Fehler: Wertliste für Merkmal PAR_CABNAVVNAME kann nicht gelesen werden
        message I005(/WATP/NCONTSERVICE) with PAR_CABNAVVNAME.
      endif.
    else.
*     Fehler: Merkmal PAR_CABNAVVNAME nicht korrekt eingestellt
      message I006(/WATP/NCONTSERVICE) with PAR_CABNAVVNAME.
    endif.
  else.
*   Das Merkmal zur Beschreibung der möglichen Abfälle ist nicht vorhanden!
    message I007(/WATP/NCONTSERVICE).
  endif.
  if PAR_MATNR is not initial.
    LINFO = /WATP/CL_BO_MARA=>READ_MARA( PAR_MATNR = PAR_MATNR ).
    if LINFO is initial.
      clear PAR_OK.
*     Fehler: Das Material PAR_MATNR existiert nicht im Materialstamm
      message I013(/WATP/NCONTSERVICE) with PAR_MATNR.
    endif.
  else.
    clear PAR_OK.
*   Die Materialnummer fehlt!
    message I012(/WATP/NCONTSERVICE).
  endif.
endmethod.


method CHECK_CABNAVV_NAME .
* Merkmale vergleichen
  CHECK_CABNAVV_NAME_INT(
     exporting
      PAR_CABNAVVNAME = PAR_CABNAVVNAME
  ).
endmethod.


method CHECK_CABNAVV_NAME_INT .
  if DATAREF->CABN_NAME <> PAR_CABNAVVNAME.
*   Achtung: Der Name des aktuellen Merkmals PAR_CABNAVVNAME stimmt nicht mit dem gespeicherten Merkmal DATAREF->CABN_NAME überein!
    message I010(/WATP/NCONTSERVICE) with PAR_CABNAVVNAME DATAREF->CABN_NAME.
  endif.
endmethod.


  method CONSTRUCTOR.
    SUPER->CONSTRUCTOR( ).
    FOBJTYPE = COT_CS_CABAVV.   "Objekttypbezeichnung für Objektkonfiguration
  endmethod.


  method COPY_FROM_BO.
    data:
      FOLDCALLBACK type ref to /WATP/IF_BO_CALLBACK_BASE.
*      LKEY type /WATP/SCSCABNAVV_KEY.

    if PAR_BO_SRC is not initial.
      FOLDCALLBACK = PAR_BO_SRC->GET_CALLBACK( ).
      PAR_BO_SRC->SET_CALLBACK( exporting PAR_INTERFACE = FCALLBACK ).
      try.
*          LOCK_FOR_EDIT( ).
          PAR_BO_SRC->LOCK_FOR_EDIT( ).
          DATAREF->DATA = PAR_BO_SRC->DATAREF->DATA.
          " diese Sonderlocke an diesem BO -> todo: dieser Quatsch raus aus prepare_insert. -> führt zu doppelten Einträgen beim COPY.
*           Das hat das DETAIL zu klären -> USED, SAVE_AVV und nicht der Hauptsatz.
*          data:
*             LCABNAVVS  type                   /WATP/PCSBO_CABAVVD.
**         Detaildatensätze laut Merkmal erzeugen:
*          CHECK_CABNAVV_INT(
*            exporting
*              PAR_CABNAVVNAME = DATAREF->CABN_NAME
*              PAR_MATNR       = DATAREF->MATNR
*            importing
**             PAR_OK = LOK
*              PAR_CABNAVVS    = LCABNAVVS
*          ).
*          AVV_FILL_UP( PAR_CABNAVVS = LCABNAVVS ).


          PAR_BO_SRC->COPY_DETAILS( exporting PAR_TARGET = ME ).
          FMATLIST = PAR_BO_SRC->FMATLIST.
          FCABNAVVCCCONFIG = PAR_BO_SRC->FCABNAVVCCCONFIG.
          FCABNAVVCCCONFIGKEY = PAR_BO_SRC->FCABNAVVCCCONFIGKEY.
          PAR_BO_SRC->UNLOCK_FOR_EDIT( ).
*          UNLOCK_FOR_EDIT( ).
        cleanup.
*          UNLOCK_FOR_EDIT( ).
          PAR_BO_SRC->UNLOCK_FOR_EDIT( ).
          PAR_BO_SRC->SET_CALLBACK( exporting PAR_INTERFACE = FOLDCALLBACK ).
      endtry.
      PAR_BO_SRC->SET_CALLBACK( exporting PAR_INTERFACE = FOLDCALLBACK ).
    endif.
  endmethod.


  method DATA_LOADED.
    " Für Erweiterungszwecke überschrieben. Bitte stehen lassen!
    SUPER->DATA_LOADED( exporting PAR_FAILED = PAR_FAILED ).
    AVV_JOIN_UP( ).
  endmethod.


  method DATA_NEWED.
    " Für Erweiterungszwecke überschrieben. Bitte stehen lassen!
    SUPER->DATA_NEWED( exporting PAR_FAILED = PAR_FAILED ).
  endmethod.


method INSERT_DATA_SPEC.
  " Für Erweiterungszwecke überschrieben. Bitte stehen lassen!
  SUPER->INSERT_DATA_SPEC( ).
endmethod.


method LOAD_DATA_SPEC.
  " Für Erweiterungszwecke überschrieben. Bitte stehen lassen!
  SUPER->LOAD_DATA_SPEC( exporting PAR_KEY = PAR_KEY PAR_KEYONLY = PAR_KEYONLY ).
endmethod.


method PREPARE_INSERT.
  data:
    LCABNAVVS  type                   /WATP/PCSBO_CABAVVD.
  SUPER->PREPARE_INSERT( ).

* Detaildatensätze laut Merkmal erzeugen:  FW: Auch beim Kopieren???
  CHECK_CABNAVV_INT(
    exporting
      PAR_CABNAVVNAME = DATAREF->CABN_NAME
      PAR_MATNR       = DATAREF->MATNR
    importing
*     PAR_OK = LOK
      PAR_CABNAVVS    = LCABNAVVS
  ).
  AVV_FILL_UP( PAR_CABNAVVS = LCABNAVVS ).
endmethod.


method SAVE_DATA_SPEC.
  " Für Erweiterungszwecke überschrieben. Bitte stehen lassen!
  SUPER->SAVE_DATA_SPEC( ).
endmethod.


  method SET_SVOBJ.
    SUPER->SET_SVOBJ(
      exporting
        PAR_SVOBJ             = PAR_SVOBJ
        PAR_CABN_NAME         = PAR_CABN_NAME
        PAR_REFRESH_MATERIALS = PAR_REFRESH_MATERIALS
      ).

    if PAR_REFRESH_MATERIALS is not initial.
*      AVV_CLEAR( ). ->done in super( )
      BOOK_AVV_FILL_UP( ).
      AVV_JOIN_UP( ).
    endif.
  endmethod.
ENDCLASS.
