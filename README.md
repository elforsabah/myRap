class /PLCE/CL_PD_PROC_WASTE_ORDERS definition
  public
  create public .

public section.

  interfaces IF_BADI_INTERFACE .
  interfaces /PLCE/IF_PD_SO_SRVC_PROC .

  types:
    begin of TS_SERVICE_ORDER_ITEM.
        include type /PLCE/SSERVICEORDERITEM.
    types: end of TS_SERVICE_ORDER_ITEM .
  types:
    begin of TS_SERVICE_STATUS,
        ServiceUUID    type  /PLCE/R_PDService-ServiceUUID,
        ServiceId      type /PLCE/R_PDService-ServiceId,
        PlanningStatus type  /PLCE/R_PDService-PlanningStatus,
        ReferenceId     type  /PLCE/R_PDService-ReferenceId,
        ReferenceInternalId type  /PLCE/R_PDService-ReferenceInternalId,
      end of TS_SERVICE_STATUS .
  types:
    TP_SERVICE_STATUS type standard table of TS_SERVICE_STATUS with default key
           with non-unique sorted key SOI components ReferenceId ReferenceInternalId .
  types:
*  types:
*    TP_SERVICE_ORDER_ITEM type standard table of TS_SERVICE_ORDER_ITEM with default key .
*  types:
*    TP_SERVICE_ORDER  type standard table of I_ServiceOrderTP with default key .
    TP_SERVICE_TMPL type standard table of /PLCE/R_PDServiceTemplate with default key .
  types:
    TP_SERVICE_TASK_TMPL type standard table of /PLCE/R_PDServiceTaskTmpl with default key .
  types:
    TP_SERVICE type standard table of /PLCE/R_PDService with default key .
  types:
    TP_SERVICE_TASK type standard table of /PLCE/R_PDSERVICETASK with default key .
  types:
    begin of TS_MODIFY_SERVICE_TASK_EXT,
      SERVICETASK type /PLCE/R_PDSERVICETASK,
      EXTUNIVERSAL type /PLCE/R_PDSERVICETASKEXTUNI,
      EXTWASTE     type /PLCE/R_PDSERVICETASKEXTWR,
    end of TS_MODIFY_SERVICE_TASK_EXT .
  types:
    begin of TS_MODIFY_SERVICE_NOTE_EXT,
      SERVICE_NOTE type /PLCE/R_PDServiceNote,
    end of TS_MODIFY_SERVICE_NOTE_EXT .
  types:
    begin of TS_MODIFY_SERVICE_ATTACHEMNT,
      ATTACHMENT type /PLCE/R_PDServiceAttachment,
    end of TS_MODIFY_SERVICE_ATTACHEMNT .
  types:
    TP_MODIFY_SERVICE_TASK_EXT type standard table of TS_MODIFY_SERVICE_TASK_EXT with default key .
  types:
    TP_MODIFY_SERVICE_NOTE_EXT type standard table of TS_MODIFY_SERVICE_NOTE_EXT with default key .
  types:
    TP_MODIFY_SERVICE_ATTACHEMNT type standard table of TS_MODIFY_SERVICE_ATTACHEMNT with default key .
  types:
    begin of TS_SERVICE_TASK_EXT,
        SERVICETASK type ref to /PLCE/R_PDSERVICETASK,
        EXTUNIVERSAL type ref to /PLCE/R_PDSERVICETASKEXTUNI,
        EXTWASTE     type ref to /PLCE/R_PDSERVICETASKEXTWR,
      end of TS_SERVICE_TASK_EXT .
  types:
    TP_SERVICE_TASK_EXT type standard table of TS_SERVICE_TASK_EXT with default key .
  types:
    begin of TP_SERVICE_EXT,
        SERVICE      type ref to /PLCE/R_PDSERVICE,
        EXTUNIVERSAL type ref to /PLCE/R_PDServiceExtUNI,
        EXTWASTE     type ref to /PLCE/R_PDServiceExtWR,
      end of TP_SERVICE_EXT .
  types:
    begin of TS_MODIFY_SERVICE,
        CID            type ABP_BEHV_CID,
        SERVICE        type /PLCE/R_PDSERVICE,
        EXTUNIVERSAL   type /PLCE/R_PDServiceExtUNI,
        EXTWASTE       type /PLCE/R_PDServiceExtWR,
        T_SERVICE_TASK type TP_SERVICE_TASK,
      end of TS_MODIFY_SERVICE .
  types:
    TP_MODIFY_SERVICE type sorted table of TS_MODIFY_SERVICE with unique key CID .
  types:
    begin of TS_MODIFY_SERVICE_EXT,
        CID            type ABP_BEHV_CID,
        SERVICE        type /PLCE/R_PDSERVICE,
        EXTUNIVERSAL   type /PLCE/R_PDServiceExtUNI,
        EXTWASTE       type /PLCE/R_PDServiceExtWR,
        ATTACHMENTS    type TP_MODIFY_SERVICE_ATTACHEMNT,
        T_SERVICE_TASK type TP_MODIFY_SERVICE_TASK_EXT,
        T_SERVICE_NOTES type TP_MODIFY_SERVICE_NOTE_EXT,
      end of TS_MODIFY_SERVICE_EXT .
  types:
    TP_MODIFY_SERVICE_EXT type sorted table of TS_MODIFY_SERVICE_EXT with unique key CID .
  types:
    begin of TS_ACTION,
        ACTION             type /PLCE/PDACTION,
        T_SERVICE_TEMPLATE type TP_SERVICE_TMPL,
      end of TS_ACTION .
  types:
    TP_ACTION type sorted table of TS_ACTION with unique key ACTION .
  types:
    begin of TS_SERVICE_TYPE,
        ServiceType             type /PLCE/PDSERVICE_TYPE,
        T_SERVICE_TASK_TEMPLATE type TP_SERVICE_TASK_TMPL,
      end of TS_SERVICE_TYPE .
  types:
    TP_SERVICE_TYPE type sorted table of TS_SERVICE_TYPE with unique key ServiceType .
  types:
    begin of TS_PROFILE,
        PROFILE        type /PLCE/PROFILE,
        T_ACTION       type TP_ACTION,
        T_SERVICE_TYPE type TP_SERVICE_TYPE,
      end of TS_PROFILE .
  types:
    TP_PROFILE type sorted table of TS_PROFILE with unique key PROFILE .
  types:
    TR_EXTERNAL_ID      type range of /PLCE/PDEXTERNAL_ID .
  types:
    TR_EXTERNAL_ITEM_ID type range of /PLCE/PDEXTERNAL_ITEM_ID .
  types:
    TR_SERVICE_STATUS   type range of /PLCE/PDSERVICE_STATUS .

  constants:
    begin of C_MESSAGE_LOG_BASE,
        LOG_OBJECT    type CL_BALI_HEADER_SETTER=>TY_OBJECT value '/PLCE/PD',
        LOG_SUBOBJECT type CL_BALI_HEADER_SETTER=>TY_SUBOBJECT value '/PLCE/PD_SO_TO_SRVC',
        LOG_EXTID     type CL_BALI_HEADER_SETTER=>TY_EXTERNAL_ID value 'ServiceOrderToService',
      end of C_MESSAGE_LOG_BASE .

  methods CONSTRUCTOR
    importing
      !IO_MSGLOG type ref to /PLCE/CL_APPLLOG_HELPER optional
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods GET_PROFILE
    returning
      value(RV_PROFILE) type /PLCE/PDPROFILE .
  methods PROCESS_WASTEORDERS
    importing
      !IV_PROFILE type /PLCE/PDPROFILE
      !IT_WASTEORDER type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER
      !IO_MSGLOG type ref to /PLCE/CL_APPLLOG_HELPER optional
    exporting
      !ET_SRV_MAPPING type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING
      !ET_SRV_KEY type /PLCE/PPDSRV_KEY
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods SET_PROFILE
    importing
      !IV_PROFILE type /PLCE/PDPROFILE
    raising
      /PLCE/CX_BASEEXCEPTION .
protected section.

  types:
    begin of TS_SOI_AVC_VALUES,
        ServiceOrderItemUUID type SYSUUID_X16,
        AVC_VALUES           type /PLCE/CL_SEMANTIC_AVC_MISC=>TS_AVC_VALUES,
      end of TS_SOI_AVC_VALUES .

  data FO_MSGLOG type ref to /PLCE/CL_APPLLOG_HELPER .
  data FO_ACCESS_POOL type ref to /PLCE/CL_SEM_ACS_POOL .
  data FO_MAPPER type ref to /PLCE/CL_SEMANTIC_MAPPER .
  data FV_PROFILE type /PLCE/PDPROFILE .
  data FV_CID_NUMBER type SY-TABIX .
  data FR_PROFILE type ref to TS_PROFILE .
  data FT_PROFILE type TP_PROFILE .
  data FR_WASTE_ORDER_ITEM type ref to /PLCE/CL_PD_IMP_WASTE_ORDERS=>TS_WASTEORDER .
  data FR_SERVICE type ref to TP_SERVICE_EXT .
  data FR_SERVICE_TASK type ref to TS_SERVICE_TASK_EXT .
  data FT_SERVICE_TO_CREATE type TP_MODIFY_SERVICE_EXT .
  data FT_WASTE_ORDER_ITEM_CREA type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER .
  data FT_WASTE_ORDER_ITEM_UPD type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER .

  methods COMPOSE_TOURS
    importing
      !IT_SRV_KEY type /PLCE/PPDSRV_KEY
      !IT_SRV_MAPPING type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods CREATE_SERVICES
    changing
      !CT_SRV_KEY type /PLCE/PPDSRV_KEY optional
      !CT_SRV_MAPPING type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING optional
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods FILL_SERVICES
    importing
      !IT_WASTE_ORDER_ITEM_DATA type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods FILL_SERVICE_DATA
    importing
      !IS_SERVICE_TEMPLATE type LINE OF TP_SERVICE_TMPL optional
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods FILL_SERVICE_TASK_DATA
    importing
      !IS_SRVC_TSK_TMPL type LINE OF TP_SERVICE_TASK_TMPL optional
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods GET_ACTION
    importing
      !IV_PRODUCT type MATNR optional
      !IV_SERVICE_ORDER_ITEM_UUID type SYSUUID_X16 optional
    returning
      value(RV_ACTION) type /PLCE/PDACTION
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods GET_NEXT_CID
    returning
      value(RV_CID) type ABP_BEHV_CID
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods INIT_MESSAGE_LOG
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods PROCESS_SERVICES
    changing
      !CT_SRV_KEY type /PLCE/PPDSRV_KEY optional
      !CT_SRV_MAPPING type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING optional
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods PROCESS_UPDATE
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods READ_SERVICE_STATUS_BY_SOIKEYS
    returning
      value(RT_SERVICE_STATUS) type TP_SERVICE_STATUS .
  methods READ_SERVICE_TASK_TEMPLATES
    importing
      !IV_SERVICETYPE type /PLCE/PDSERVICE_TYPE
    returning
      value(RT_SERVICE_TASK_TMPL) type TP_SERVICE_TASK_TMPL
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods READ_SERVICE_TEMPLATES
    importing
      !IV_ACTION type /PLCE/PDACTION
    returning
      value(RT_SERVICE_TMPL) type TP_SERVICE_TMPL
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods SAVE_SERVICE
    exporting
      !ET_SERVICE_KEY type /PLCE/PPDSRV_KEY
      !ET_SRV_MAPPING type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods SET_DATA
    importing
      !IT_WASTE_ORDER type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WASTEORDER
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods SET_SERVICE_ORDER_ITEM_KEYS
    importing
      !IT_SERVICE_ORDER_ITEM_KEY type /PLCE/PPDSOI_KEY
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods UPDATE_SERVICES
    changing
      !CT_SRV_KEY type /PLCE/PPDSRV_KEY optional
      !CT_SRV_MAPPING type /PLCE/CL_PD_IMP_WASTE_ORDERS=>TP_WDOI_SRV_MAPPING optional
    raising
      /PLCE/CX_BASEEXCEPTION .
private section.

  methods INIT_CID .
ENDCLASS.



CLASS /PLCE/CL_PD_PROC_WASTE_ORDERS IMPLEMENTATION.


  method /PLCE/IF_PD_SO_SRVC_PROC~PROCESS_SERVICES.
*    PROCESS_SERVICES(
*      exporting
*        IV_PROFILE  = IV_PROFILE
*        IT_SOI_KEY  = IT_SOI_KEY
*        IO_MSGLOG   = IO_MSGLOG
*      changing
*        CT_SRVC_KEY = CT_SRVC_KEY
*    ).
  endmethod.


  method COMPOSE_TOURS.

*** Find Tours
*** Create missing tours
*** assign services

    types:
      begin of _TSRVSEQ,
        SERVICE_UUID type /PLCE/PDSERVICE_UUID,
        TOURSEQENCE  type /PLCE/SEQUENCE_NUMBER,
      end of _TSRVSEQ.
    types:
      begin of _TTOUR_UPD,
        UPD_ID             type I,
        TOURTEMPLATE       type /PLCE/PDTOUR_TEMPLATE,
        STARTDATE          type /PLCE/DATE,
        TOURUUID           type /PLCE/PDTOUR_UUID,
        TOURID             type /PLCE/PDTOUR_ID,
        DURATION_MAX       type /PLCE/PDMAX_TOUR_DURATION,
        DURATION_MAX_UNIT  type /PLCE/DURATION_UNIT,
        DURATION_USED      type /PLCE/DURATION,
        DURATION_USED_UNIT type /PLCE/DURATION_UNIT,
        SERVICES           type standard table of _TSRVSEQ with default key,
      end of _TTOUR_UPD.
    types:
      begin of _TTOUR_PROF,
        TOURUUID type /PLCE/PDTOUR_UUID,
        PROFILE   type /PLCE/PDPROFILE,
      end of _TTOUR_PROF.

    types:
      begin of _TSERVICE,
        SERVICE_UUID  type /PLCE/PDSERVICE_UUID,
        SERVICEID     type /PLCE/PDSERVICE_ID,
        POBJNR        type C length 30,
        TOUR_TEMPLATE type /PLCE/PDTOUR_TEMPLATE,
        TOURSEQUENCE  type /PLCE/SEQUENCE_NUMBER,
        DURATION      type /PLCE/DURATION,
        DURATION_UNIT type /PLCE/DURATION_UNIT,
        REQUESTEDDATE type /PLCE/DATE,
        PROFILE       type /PLCE/PDPROFILE,
        ASSIGNED      type /PLCE/INDICATOR_FLAG,
      end of _TSERVICE.

    types:
      begin of _TROUTE,
        ROUTE      type /PLCE/PDTOUR_TEMPLATE,
        ORDER_DATE type /PLCE/DATE,
      end of _TROUTE,
      begin of _TROUTE_TEMP,
        ROUTE      type /PLCE/PDEXT_TOUR_TEMPLATE_ID,
        ORDER_DATE type /PLCE/DATE,
      end of _TROUTE_TEMP.

    types:
      begin of _TRTOUR,
        TOUR_UUID     type /PLCE/PDTOUR_UUID,
        TOURID        type /PLCE/PDTOUR_ID,
        TOUR_TEMPLATE type /PLCE/PDTOUR_TEMPLATE,
        STARTDATE     type /PLCE/DATE,
      end of _TRTOUR.

    data:
      LROUTES        type standard table of _TROUTE,
      LROUTE_TEMPS   type standard table of _TROUTE_TEMP,
      LROUTE_TEMPREF type ref to _TROUTE_TEMP,
      LROUTEREF      type ref to _TROUTE,
      LROUTES_NEW    type standard table of _TROUTE,
*      LTOURS      type standard table of _TRTOUR
      LTOURS         type standard table of _TTOUR_UPD
        with non-unique sorted key SS_ROUTE components TOURTEMPLATE STARTDATE,
      LTOURPROFILES  type standard table of _TTOUR_PROF,
      LTOURREF       type ref to _TTOUR_UPD,
      LTOURS_CREATE  type standard table of _TTOUR_UPD.


    types:
      begin of _TTOURREMPL,
        TOURTEMPLATE            type /PLCE/PDTOUR_TEMPLATE,
        EXTERNALTOURTEMPLATEID  type /PLCE/PDEXT_TOUR_TEMPLATE_ID,
        MAXIMUMTOURDURATION     type /PLCE/PDMAX_TOUR_DURATION,
        MAXIMUMTOURDURATIONUNIT type /PLCE/PDMAX_TOUR_DURATION_UNIT,
        DURATION_USED           type /PLCE/PDMAX_TOUR_DURATION,
      end of _TTOURREMPL.

    data: LTOURTEMPLS   type standard table of _TTOURREMPL,
          LTOURTEMPLREF type ref to _TTOURREMPL.



    LROUTES = corresponding #( FT_WASTE_ORDER_ITEM_CREA ).
    LROUTES = corresponding #( base ( LROUTES ) FT_WASTE_ORDER_ITEM_UPD ). " Really ?
    sort LROUTES.
    delete adjacent duplicates from LROUTES.
    delete LROUTES where ROUTE is initial.

    LROUTE_TEMPS = corresponding #( FT_WASTE_ORDER_ITEM_CREA ).
    LROUTE_TEMPS = corresponding #( base ( LROUTE_TEMPS ) FT_WASTE_ORDER_ITEM_UPD ). " Really ?
    sort LROUTE_TEMPS.
    delete adjacent duplicates from LROUTE_TEMPS.
    delete LROUTE_TEMPS where ROUTE is initial.

    if LINES( LROUTE_TEMPS ) is not initial.
      select * from /PLCE/R_PDTOURTEMPLATE
        for all entries in @LROUTE_TEMPS
*      where TOURTEMPLATE = @LROUTES-ROUTE
        where EXTERNALTOURTEMPLATEID = @LROUTE_TEMPS-ROUTE
          and ISACTIVE is not initial
        into corresponding fields of table @LTOURTEMPLS.
    endif.

    sort LTOURTEMPLS.
    delete adjacent duplicates from LTOURTEMPLS.
    loop at LROUTES reference into LROUTEREF.
      read table LTOURTEMPLS with key EXTERNALTOURTEMPLATEID = LROUTEREF->ROUTE reference into LTOURTEMPLREF.
      if SY-SUBRC is initial.
        LROUTEREF->ROUTE = LTOURTEMPLREF->TOURTEMPLATE .
      endif.

    endloop.


    if LINES( LROUTES ) is not initial.

      select TOURUUID, TOURID, TOURTEMPLATE, STARTDATE, MAXIMUMTOURDURATION as DURATION_MAX, CALCULATEDTOURDURATION as DURATION_USED,
        MAXIMUMTOURDURATIONUNIT as DURATION_MAX_UNIT, CALCULATEDTOURDURATIONUNIT as DURATION_USED_UNIT
        from /PLCE/R_PDTOUR
        for all entries in @LROUTES
        where TOURTEMPLATE = @LROUTES-ROUTE
          and STARTDATE =  @LROUTES-ORDER_DATE
          and TOURSTATUS = @/PLCE/IF_PD_CONSTANTS=>C_PD_TOUR_STATUS-IN_PLANNING
        order by primary key
        into corresponding fields of table @LTOURS.

      if LINES( LTOURS ) is not initial.
        select TOURUUID, PROFILE from /PLCE/R_PDTourProfile
          for all entries in @LTOURS
          where TourUuid = @LTOURS-TOURUUID
          into corresponding fields of table @LTOURPROFILES.
        sort LTOURPROFILES.
      endif.

    endif.

*** which routes have to be created?

    LROUTES_NEW = filter #( LROUTES except in LTOURS using key SS_ROUTE where ROUTE = TOURTEMPLATE and ORDER_DATE = STARTDATE ).

**** Tourtemplates lesen.

    if LINES( LROUTES ) is not initial.
*      select * from /PLCE/R_PDTOURTEMPLATE
*        for all entries in @LROUTES
**        where TOURTEMPLATE = @LROUTES-ROUTE
*        where EXTERNALTOURTEMPLATEID = @LROUTES-ROUTE
*          and ISACTIVE is not initial
*        into corresponding fields of table @LTOURTEMPLS.
*
*      sort LTOURTEMPLS.
*      delete adjacent duplicates from LTOURTEMPLS.


      if LINES( LTOURTEMPLS ) is not initial.
        select * from /PLCE/R_PDTOURTEMPLATETASK
          for all entries in @LTOURTEMPLS
          where TOURTEMPLATE = @LTOURTEMPLS-TOURTEMPLATE
          into table @data(LTOURTASKS).

        if SY-SUBRC is initial.

          loop at LTOURTEMPLS reference into LTOURTEMPLREF.
            loop at LTOURTASKS reference into data(LTOURTASKREF) where TOURTEMPLATE = LTOURTEMPLREF->TOURTEMPLATE and DURATION is not initial.

              if LTOURTEMPLREF->MAXIMUMTOURDURATIONUNIT <> LTOURTASKREF->DURATIONUNIT.
                /PLCE/CL_BASE_MISC=>UNIT_CONVERSION_SIMPLE(
                  exporting
                    IV_INPUT    = LTOURTASKREF->DURATION
                    IV_UNIT_IN  = LTOURTASKREF->DURATIONUNIT
                    IV_UNIT_OUT = LTOURTEMPLREF->MAXIMUMTOURDURATIONUNIT
                  importing
                    EV_OUTPUT   = LTOURTASKREF->DURATION
                ).
*              else.

              endif.

              LTOURTEMPLREF->DURATION_USED = LTOURTEMPLREF->DURATION_USED + LTOURTASKREF->DURATION.

            endloop.
          endloop.

        endif.

      endif.


    endif.

***  erzeugte Services lesen.
    if LINES( IT_SRV_KEY ) is not initial.

      data:
        LSERVICETASKDATAS type standard table of _TSERVICE,
        LSERVICEDATAS     type standard table of _TSERVICE,
        LDURATION         type /PLCE/DURATION.


      select SRVTSK~SERVICETASKUUID, " dummy to prevent grouping
        SRV~SERVICEUUID as SERVICE_UUID, SRV~SERVICEID,  SRV~REQUESTEDDATE, SRV~REFERENCEINTERNALID as POBJNR,
        SRVTSK~DURATION, SRVTSK~DURATIONUNIT as DURATION_UNIT, SRV~PROFILE
        from /PLCE/R_PDSERVICE as SRV
        inner join /PLCE/R_PDSERVICETASK as SRVTSK
          on SRV~SERVICEUUID = SRVTSK~SERVICEUUID
        for all entries in @IT_SRV_KEY
        where SRV~SERVICEUUID = @IT_SRV_KEY-SERVICE_UUID
      into corresponding fields of table @LSERVICETASKDATAS.

*** Build Servicedata!!

      loop at LSERVICETASKDATAS reference into data(LGROUP) group by LGROUP->SERVICE_UUID reference into data(LGROUPREF).
        insert initial line into table LSERVICEDATAS reference into data(LSERVICEREF).


        loop at group LGROUPREF reference into data(LDATAREF) where DURATION is not initial.

          if LSERVICEREF->* is initial.
            LSERVICEREF->SERVICE_UUID = LDATAREF->SERVICE_UUID.
            LSERVICEREF->SERVICEID = LDATAREF->SERVICEID.
            LSERVICEREF->POBJNR = LDATAREF->POBJNR.
            LSERVICEREF->REQUESTEDDATE = LDATAREF->REQUESTEDDATE.
            LSERVICEREF->PROFILE = LDATAREF->PROFILE.
*            LSERVICEREF->DURATION = LGROUP->DURATION.
*            LSERVICEREF->DURATION_UNIT = LGROUP->DURATION_UNIT.

            read table FT_WASTE_ORDER_ITEM_CREA reference into FR_WASTE_ORDER_ITEM with table key POBJNR = LSERVICEREF->POBJNR.
            if SY-SUBRC is initial.
              read table LTOURTEMPLS with key EXTERNALTOURTEMPLATEID = FR_WASTE_ORDER_ITEM->ROUTE reference into LTOURTEMPLREF.
              if SY-SUBRC is initial.
*                LSERVICEREF->TOUR_TEMPLATE = FR_WASTE_ORDER_ITEM->ROUTE.
*                LSERVICEREF->TOURSEQUENCE = FR_WASTE_ORDER_ITEM->ROUTE_SEQUENCE.
                LSERVICEREF->TOUR_TEMPLATE = LTOURTEMPLREF->TOURTEMPLATE.
                LSERVICEREF->TOURSEQUENCE = FR_WASTE_ORDER_ITEM->ROUTE_SEQUENCE.
              endif.
            endif.

            if LSERVICEREF->TOUR_TEMPLATE is not initial.
              read table LTOURTEMPLS reference into LTOURTEMPLREF with key TOURTEMPLATE = LSERVICEREF->TOUR_TEMPLATE binary search.
              if SY-SUBRC is not initial.
                clear LTOURTEMPLREF.
              endif.
            else.
              clear LTOURTEMPLREF.
            endif.

          endif.

          clear:
            LDURATION.

          if LSERVICEREF->DURATION is initial.
            if LTOURTEMPLREF is initial or LTOURTEMPLREF->MAXIMUMTOURDURATIONUNIT is initial.
              LSERVICEREF->DURATION = LDATAREF->DURATION.
              LSERVICEREF->DURATION_UNIT = LDATAREF->DURATION_UNIT.
            else.

              LSERVICEREF->DURATION_UNIT = LTOURTEMPLREF->MAXIMUMTOURDURATIONUNIT.
              /PLCE/CL_BASE_MISC=>UNIT_CONVERSION_SIMPLE(
                exporting
                  IV_INPUT    = LDATAREF->DURATION
                  IV_UNIT_IN  = LDATAREF->DURATION_UNIT
                  IV_UNIT_OUT = LSERVICEREF->DURATION_UNIT
                importing
                  EV_OUTPUT   = LSERVICEREF->DURATION
              ).

            endif.
          else.
            if LSERVICEREF->DURATION_UNIT <> LDATAREF->DURATION_UNIT.
              /PLCE/CL_BASE_MISC=>UNIT_CONVERSION_SIMPLE(
                exporting
                  IV_INPUT    = LDATAREF->DURATION
                  IV_UNIT_IN  = LDATAREF->DURATION_UNIT
                  IV_UNIT_OUT = LSERVICEREF->DURATION_UNIT
                importing
                  EV_OUTPUT   = LDURATION
              ).

              LSERVICEREF->DURATION = LSERVICEREF->DURATION + LDURATION.
            else.
              LSERVICEREF->DURATION = LSERVICEREF->DURATION + LDATAREF->DURATION.
            endif.
          endif.
        endloop.

        clear:
          LSERVICEREF.
      endloop.

      sort LSERVICEDATAS by SERVICEID.

    endif.

*** Services in die bestehenden Routen einfügen... wo es passt.
*** übrig gebliebene Services (not assigned) auf neue Touren verteilen... (auf MAX Duration achten)

*** zuerst touren auffüllen.
    data:
      LSRVKEY  type /PLCE/SPDSRV_KEY,
      LSRVTSEQ type _TSRVSEQ.

    loop at LTOURS reference into LTOURREF.

      loop at LSERVICEDATAS reference into LSERVICEREF
        where TOUR_TEMPLATE = LTOURREF->TOURTEMPLATE
          and REQUESTEDDATE = LTOURREF->STARTDATE
          and ASSIGNED is initial. " todo using KEY

        clear:
          LDURATION,
          LSRVKEY.
*** does the service profile match to the tour profiles?
        read table LTOURPROFILES with key TOURUUID = LTOURREF->TOURUUID PROFILE = LSERVICEREF->PROFILE transporting no fields binary search.
        if SY-SUBRC is not initial.
          continue.
        endif.

        if LTOURREF->DURATION_MAX is not initial.
          if LTOURREF->DURATION_MAX_UNIT is not initial and LSERVICEREF->DURATION_UNIT is not initial and LTOURREF->DURATION_MAX_UNIT <> LSERVICEREF->DURATION_UNIT.
            /PLCE/CL_BASE_MISC=>UNIT_CONVERSION_SIMPLE(
              exporting
                IV_INPUT    = LSERVICEREF->DURATION
                IV_UNIT_IN  = LSERVICEREF->DURATION_UNIT
                IV_UNIT_OUT = LTOURREF->DURATION_MAX_UNIT
              importing
                EV_OUTPUT   = LDURATION
            ).

          else.
            LDURATION = LSERVICEREF->DURATION.
          endif.

        endif.

        if LTOURREF->DURATION_MAX is not initial and  LTOURREF->DURATION_MAX < ( LTOURREF->DURATION_USED + LDURATION ).
          insert value #( ROUTE = LSERVICEREF->TOUR_TEMPLATE ORDER_DATE = LSERVICEREF->REQUESTEDDATE ) into table LROUTES_NEW.
          continue. " service passt nicht.
        else.
*          LSRVKEY-SERVICE_UUID = LSERVICEREF->SERVICE_UUID. "? Route sequence?
          LSRVTSEQ-SERVICE_UUID = LSERVICEREF->SERVICE_UUID. "? Route sequence?
          LSRVTSEQ-TOURSEQENCE  = LSERVICEREF->TOURSEQUENCE. "? Route sequence?
*          insert LSRVKEY into table LTOURREF->SERVICES.
          insert LSRVTSEQ into table LTOURREF->SERVICES.
          LTOURREF->DURATION_USED = LTOURREF->DURATION_USED + LDURATION.
          LSERVICEREF->ASSIGNED = 'X'.
        endif.

      endloop.

    endloop.


***** new tours
    sort LROUTES_NEW.
    delete adjacent duplicates from LROUTES_NEW.

    loop at LROUTES_NEW reference into LROUTEREF.

      read table LTOURTEMPLS reference into LTOURTEMPLREF with key TOURTEMPLATE = LROUTEREF->ROUTE binary search.
      if SY-SUBRC is initial. " sollte immer klappen

        insert initial line into table LTOURS_CREATE reference into LTOURREF.
        LTOURREF->TOURTEMPLATE = LTOURTEMPLREF->TOURTEMPLATE.
        LTOURREF->DURATION_MAX = LTOURTEMPLREF->MAXIMUMTOURDURATION.
        LTOURREF->DURATION_MAX_UNIT = LTOURTEMPLREF->MAXIMUMTOURDURATIONUNIT.
        LTOURREF->STARTDATE = LROUTEREF->ORDER_DATE.
        LTOURREF->DURATION_USED = LTOURTEMPLREF->DURATION_USED.

        loop at LSERVICEDATAS reference into LSERVICEREF
                where TOUR_TEMPLATE = LTOURREF->TOURTEMPLATE
                  and REQUESTEDDATE = LTOURREF->STARTDATE
                  and ASSIGNED is initial. " todo using KEY

          clear:
            LDURATION,
            LSRVKEY.

          if LTOURREF->DURATION_MAX is not initial.
            if LTOURREF->DURATION_MAX_UNIT is not initial and LSERVICEREF->DURATION_UNIT is not initial and LTOURREF->DURATION_MAX_UNIT <> LSERVICEREF->DURATION_UNIT.
              /PLCE/CL_BASE_MISC=>UNIT_CONVERSION_SIMPLE(
                exporting
                  IV_INPUT    = LSERVICEREF->DURATION
                  IV_UNIT_IN  = LSERVICEREF->DURATION_UNIT
                  IV_UNIT_OUT = LTOURREF->DURATION_MAX_UNIT
                importing
                  EV_OUTPUT   = LDURATION
              ).

            else.
              LDURATION = LSERVICEREF->DURATION.
            endif.

          endif.

          if LTOURREF->DURATION_MAX is not initial and LTOURREF->DURATION_MAX < ( LTOURREF->DURATION_USED + LDURATION ).
            if LSERVICEREF->DURATION <= LTOURREF->DURATION_MAX. " passt der service überhaupt in eine Tour? dann nochmal ansonsten nop.
              insert value #( ROUTE = LSERVICEREF->TOUR_TEMPLATE ORDER_DATE = LSERVICEREF->REQUESTEDDATE ) into table LROUTES_NEW.
            endif.
            continue. " service passt nicht.
          else.
            LSRVKEY-SERVICE_UUID = LSERVICEREF->SERVICE_UUID. "? Route sequence?
            LSRVTSEQ-SERVICE_UUID = LSERVICEREF->SERVICE_UUID.
            LSRVTSEQ-TOURSEQENCE = LSERVICEREF->TOURSEQUENCE.
*            insert LSRVKEY into table LTOURREF->SERVICES.
            insert LSRVTSEQ into table LTOURREF->SERVICES.
            LTOURREF->DURATION_USED = LTOURREF->DURATION_USED + LDURATION.
            LSERVICEREF->ASSIGNED = 'X'.
          endif.

        endloop.

        if SY-SUBRC is not initial.
**          delete LROUTES_NEW.
        endif.

      endif.
    endloop.


*    break-point.



    data:
      LTOURCREATES type table for action import /PLCE/R_PDSERVICE~CREATETOUR,
      LTOURASSIGNS type table for action import /PLCE/R_PDSERVICE~ASSIGNTOUR,
      LTOURCREATE  like line of LTOURCREATES,
      LTOURASSIGN  like line of LTOURASSIGNS,
      LFAILED      type /PLCE/INDICATOR_FLAG.


    loop at LTOURS_CREATE reference into LTOURREF.
      clear LTOURCREATE.

      LTOURCREATE-%PARAM-TOUR_TEMPLATE = LTOURREF->TOURTEMPLATE.
      LTOURCREATE-%PARAM-START_DATE = LTOURREF->STARTDATE.
*      LTOURCREATE- = LTOURREF->STARTDATE.

      loop at LTOURREF->SERVICES into LSRVTSEQ.
        LTOURCREATE-SERVICEUUID = LSRVTSEQ-SERVICE_UUID.
        LTOURCREATE-%KEY-SERVICEUUID = LSRVTSEQ-SERVICE_UUID.
        LTOURCREATE-%PARAM-SEQUENCE = LSRVTSEQ-TOURSEQENCE.
        insert LTOURCREATE into table LTOURCREATES.
      endloop.

      if SY-SUBRC is initial.
        modify entities of /PLCE/R_PDSERVICE
          entity SERVICE
            execute CREATETOUR from LTOURCREATES
            result data(LTOURESULT_CREATE)
            failed data(LTOURFAILED_CREATE)
            reported data(LTOURREPORTED_CREATE)
            mapped data(LTOURMAPPED_CREATE).

        if LTOURFAILED_CREATE is not initial.
          LFAILED = 'X'.
          rollback entities.                          "#EC CI_ROLLBACK.
          exit. "loop
        else.
          /PLCE/CL_BASE_MISC=>CHECK_RESPONSE(
            exporting
              IS_RESPONSE = LTOURREPORTED_CREATE
*            IR_PROTOCOL =                  " Interface for Protocol
              IR_LOG      = FO_MSGLOG ).

          commit entities
            responses
*            response of /PLCE/R_PDSERVICE
              failed data(FAILED_COMMIT_SRVC)
              reported data(REPORTED_COMMIT_SRVC).
        endif.
      endif.

      clear:
        LTOURCREATES.

    endloop.

    if LFAILED is initial.
      loop at LTOURS reference into LTOURREF.
        clear: LTOURASSIGN.
        LTOURASSIGN-%PARAM-TOURUUID = LTOURREF->TOURUUID.
        LTOURASSIGN-%PARAM-TOURID = LTOURREF->TOURID.

        loop at LTOURREF->SERVICES into LSRVTSEQ.
          LTOURASSIGN-SERVICEUUID = LSRVTSEQ-SERVICE_UUID.
          LTOURASSIGN-%KEY-SERVICEUUID = LSRVTSEQ-SERVICE_UUID.
          LTOURASSIGN-%PARAM-SEQUENCE = LSRVTSEQ-TOURSEQENCE.
          insert LTOURASSIGN into table LTOURASSIGNS.
        endloop.
      endloop.


*    if LINES( LTOURCREATES ) is not initial.
*      modify entities of /PLCE/R_PDSERVICE
*        entity SERVICE
*          execute CREATETOUR from LTOURCREATES
*          result data(LTOURESULT_CREATE)
*          failed data(LTOURFAILED_CREATE)
*          reported data(LTOURREPORTED_CREATE)
*          mapped data(LTOURMAPPED_CREATE).
*    endif.

      if LINES( LTOURASSIGNS ) is not initial.
        modify entities of /PLCE/R_PDSERVICE
          entity SERVICE
            execute ASSIGNTOUR from LTOURASSIGNS
            result data(LTOURESULT_ASSIGN)
            failed data(LTOURFAILED_ASSIGN)
            reported data(LTOURREPORTED_ASSIGN)
            mapped data(LTOURMAPPED_ASSIGN).

        /PLCE/CL_BASE_MISC=>CHECK_RESPONSE(
          exporting
            IS_RESPONSE = LTOURREPORTED_ASSIGN
*        IR_PROTOCOL =                  " Interface for Protocol
            IR_LOG      = FO_MSGLOG ).

        if LTOURFAILED_ASSIGN is not initial.
          rollback entities.                          "#EC CI_ROLLBACK.
          FO_MSGLOG->ADD_MESSAGE(
*            IV_SEVERITY   =
            IV_ID         = '/PLCE/MC_PD_MSG'
            IV_NUMBER     = '136'
          ).
        else.
          commit entities
            response of /PLCE/R_PDSERVICE
              failed data(FAILED_COMMIT_SRVCASS)
              reported data(REPORTED_COMMIT_SRVCASS).
        endif.
      endif.
    endif.

  endmethod.


  method CONSTRUCTOR.
    if IO_MSGLOG is initial.
      INIT_MESSAGE_LOG( ).
    else.
      FO_MSGLOG = IO_MSGLOG.
    endif.

  endmethod.


  method CREATE_SERVICES.

    try.

        FILL_SERVICES( IT_WASTE_ORDER_ITEM_DATA = FT_WASTE_ORDER_ITEM_CREA ).

        SAVE_SERVICE(
          importing
            ET_SERVICE_KEY = CT_SRV_KEY
            ET_SRV_MAPPING = CT_SRV_MAPPING
        ).

*        if FT_WASTE_ORDER_ITEM is initial. "SERVICEORDERITEMRESULT
        if LINES( CT_SRV_KEY ) is initial. "SERVICEORDERITEMRESULT
          FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
            exporting
              IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-INFORMATION
              IV_ID         = '/PLCE/MC_PD_MSG'
              IV_NUMBER     = '111'
              IV_VARIABLE_1 = conv #( GET_PROFILE( ) )
          ).
        else.
          FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
            exporting
              IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-STATUS
              IV_ID         = '/PLCE/MC_PD_MSG'
              IV_NUMBER     = '130'
              IV_VARIABLE_1 = conv #( GET_PROFILE( ) )
              IV_VARIABLE_2 = |{ CONDENSE( |{ LINES( CT_SRV_KEY ) }| ) }| ).
        endif.

      catch /PLCE/CX_BASEEXCEPTION into data(LBASEEX).
        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_EXCEPTION(
          exporting
*           IV_SEVERITY  =
            IV_EXCEPTION = LBASEEX
        ).
    endtry.
  endmethod.


  method FILL_SERVICES.

    data:
        LSEQUENCE type I.

*    try.

    loop at IT_WASTE_ORDER_ITEM_DATA reference into FR_WASTE_ORDER_ITEM.
      clear LSEQUENCE.
*          if SHOULD_SERVICE_BE_CREATED( ).
      data(LV_ACTION) = GET_ACTION( ).
      if LV_ACTION is initial.
        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
          exporting
            IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
            IV_ID         = '/PLCE/MC_PD_MSG'
            IV_NUMBER     = '110'
            IV_VARIABLE_1 = conv #( FR_WASTE_ORDER_ITEM->WASTE_TYPE )
        ).
      endif.
      data(LST_TEMPLS) = READ_SERVICE_TEMPLATES( exporting IV_ACTION = LV_ACTION ).
      loop at LST_TEMPLS assigning field-symbol(<SERVICETMPL>)
        where SemanticTemplateName <> 'ST_SERVICE_NOTE'.
        insert value #( CID = GET_NEXT_CID( ) ) into table FT_SERVICE_TO_CREATE assigning field-symbol(<LS_SERVICE_TO_CREATE>).
        create data FR_SERVICE.
        FR_SERVICE->SERVICE = ref #( <LS_SERVICE_TO_CREATE>-SERVICE ).
        FR_SERVICE->EXTUNIVERSAL = ref #( <LS_SERVICE_TO_CREATE>-EXTUNIVERSAL ).
        FR_SERVICE->EXTWASTE = ref #( <LS_SERVICE_TO_CREATE>-EXTWASTE ).
*              append initial line to FT_SERVICE_TO_CREATE reference into FR_SERVICE.
        FILL_SERVICE_DATA(
          exporting
            IS_SERVICE_TEMPLATE = <SERVICETMPL>
        ).
        if LINES( FR_WASTE_ORDER_ITEM->NOTES ) is not initial.
          loop at FR_WASTE_ORDER_ITEM->NOTES reference into data(LWASTESERVICENOTEREF).
            insert initial line into table <LS_SERVICE_TO_CREATE>-T_SERVICE_NOTES reference into data(LSERVICENOTEREF).
            FO_ACCESS_POOL->SET_CONTEXT(
              IV_SEMANTIC_CONTEXT = 'EWA_WASTE_SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
              IR_DATA             = LWASTESERVICENOTEREF
            ).
            FO_ACCESS_POOL->SET_CONTEXT(
              IV_SEMANTIC_CONTEXT = 'SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
              IR_DATA             = ref #( LSERVICENOTEREF->SERVICE_NOTE )
            ).

            FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
              IV_PROFILE                = GET_PROFILE( )
              IV_SEMANTIC_TEMPLATE_NAME = 'ST_SERVICE_NOTE'
            ).

          endloop.

        endif.


        if LINES( FR_WASTE_ORDER_ITEM->ATTACHMENTS ) is not initial.
          loop at FR_WASTE_ORDER_ITEM->ATTACHMENTS reference into data(LWASTEATTACHMENTREF).
            insert initial line into table <LS_SERVICE_TO_CREATE>-ATTACHMENTS reference into data(LSERVICEATTAREF).
*                FO_ACCESS_POOL->SET_CONTEXT(
*                  IV_SEMANTIC_CONTEXT = 'EWA_WASTE_SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
*                  IR_DATA             = LWASTESERVICENOTEREF
*                ).
*                FO_ACCESS_POOL->SET_CONTEXT(
*                  IV_SEMANTIC_CONTEXT = 'SERVICE_NOTE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
*                  IR_DATA             = ref #( LSERVICENOTEREF->SERVICE_NOTE )
*                ).
*
*                FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
*                  IV_PROFILE                = GET_PROFILE( )
*                  IV_SEMANTIC_TEMPLATE_NAME = 'ST_SERVICE_NOTE'
*                ).
            LSERVICEATTAREF->ATTACHMENT-Filename = LWASTEATTACHMENTREF->FILENAME.
            LSERVICEATTAREF->ATTACHMENT-Attachment = LWASTEATTACHMENTREF->ATTACHMENT.
            LSERVICEATTAREF->ATTACHMENT-MimeType = LWASTEATTACHMENTREF->MIMETYPE.
            LSERVICEATTAREF->ATTACHMENT-CreatedByUpload = 'X'.

          endloop.

        endif.


        data(LSTT_TEMPLS) = READ_SERVICE_TASK_TEMPLATES( FR_SERVICE->SERVICE->SERVICETYPE ).
        loop at LSTT_TEMPLS assigning field-symbol(<LS_SERVICE_TASK_TEMPLATE>)
          where TaskType <> 'ADDITIONAL_SERVICE'.
          append initial line to <LS_SERVICE_TO_CREATE>-T_SERVICE_TASK assigning field-symbol(<SERVICE_TASK_TO_CREATE>).
          create data FR_SERVICE_TASK.
          FR_SERVICE_TASK->SERVICETASK = ref #( <SERVICE_TASK_TO_CREATE>-SERVICETASK ).
          FR_SERVICE_TASK->EXTUNIVERSAL = ref #( <SERVICE_TASK_TO_CREATE>-EXTUNIVERSAL ).
          FR_SERVICE_TASK->EXTWASTE = ref #( <SERVICE_TASK_TO_CREATE>-EXTWASTE ).
          FILL_SERVICE_TASK_DATA( <LS_SERVICE_TASK_TEMPLATE> ).
        endloop.
        if LINES( FR_WASTE_ORDER_ITEM->ADDITIONAL_SERVICES ) is not initial.
          LSEQUENCE = <SERVICE_TASK_TO_CREATE>-SERVICETASK-SequenceNumber.

          loop at FR_WASTE_ORDER_ITEM->ADDITIONAL_SERVICES reference into data(LADDSERVICEREF).
            loop at LSTT_TEMPLS assigning <LS_SERVICE_TASK_TEMPLATE>
              where TaskType = 'ADDITIONAL_SERVICE'.

              append initial line to <LS_SERVICE_TO_CREATE>-T_SERVICE_TASK assigning <SERVICE_TASK_TO_CREATE>.
              create data FR_SERVICE_TASK.
              FR_SERVICE_TASK->SERVICETASK = ref #( <SERVICE_TASK_TO_CREATE>-SERVICETASK ).
              FR_SERVICE_TASK->EXTUNIVERSAL = ref #( <SERVICE_TASK_TO_CREATE>-EXTUNIVERSAL ).
              FR_SERVICE_TASK->EXTWASTE = ref #( <SERVICE_TASK_TO_CREATE>-EXTWASTE ).

              FO_ACCESS_POOL->SET_CONTEXT(
                IV_SEMANTIC_CONTEXT = 'EWA_WASTE_ADDITIONAL_SERVICE' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
                IR_DATA             = LADDSERVICEREF
              ).
              FILL_SERVICE_TASK_DATA( <LS_SERVICE_TASK_TEMPLATE> ).
              LSEQUENCE = LSEQUENCE + 10.
              <SERVICE_TASK_TO_CREATE>-SERVICETASK-SequenceNumber = LSEQUENCE.

*                  FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
*                    IV_PROFILE                = GET_PROFILE( )
*                    IV_SEMANTIC_TEMPLATE_NAME = <LS_SERVICE_TASK_TEMPLATE>-SemanticTemplateName
*                  ).

            endloop.
          endloop.
        endif.
      endloop.
*          endif.
    endloop.

*      catch /PLCE/CX_BASEEXCEPTION into data(LBASEEX).
*        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_EXCEPTION(
*          exporting
**           IV_SEVERITY  =
*            IV_EXCEPTION = LBASEEX
*        ).
*    endtry.

  endmethod.


  method FILL_SERVICE_DATA.
    field-symbols:
      <FIELD_FROM> type DATA,
      <FIELD_TO>   type DATA.

*    FR_SERVICE->* = corresponding #( IS_SERVICE_TEMPLATE mapping SequenceNumber = SqncNr ).
    FR_SERVICE->SERVICE->* = corresponding #( IS_SERVICE_TEMPLATE ).

    if FR_WASTE_ORDER_ITEM is not initial.
*      FR_SERVICE->SERVICE->ReferenceId         = |{ FR_WASTE_ORDER_ITEM->ORDERNR alpha = out }/{ FR_WASTE_ORDER_ITEM->ORDER_LAUFNR }|. condense FR_SERVICE->SERVICE->ReferenceId.
      FR_SERVICE->SERVICE->ReferenceId         = |{ FR_WASTE_ORDER_ITEM->WDOITEMID alpha = out }|. condense FR_SERVICE->SERVICE->ReferenceId.
      FR_SERVICE->SERVICE->ReferenceInternalId = FR_WASTE_ORDER_ITEM->POBJNR.
*      FR_SERVICE->SERVICE->ExternalOrderType  = FR_SERVICE_ORDER_ITEM->ServiceOrderItemCategory. "ServiceDocumentItemObjectType
      FR_SERVICE->SERVICE->Profile = GET_PROFILE( ).

      FO_ACCESS_POOL->SET_CONTEXT(
        IV_SEMANTIC_CONTEXT = /PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-SERVICE
        IR_DATA             = FR_SERVICE
      ).
      FO_ACCESS_POOL->SET_CONTEXT(
        IV_SEMANTIC_CONTEXT = 'EWA_WASTE_ORDER_ITEM' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-??
        IR_DATA             = FR_WASTE_ORDER_ITEM
      ).
      FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
        IV_PROFILE                = GET_PROFILE( )
        IV_SEMANTIC_TEMPLATE_NAME = IS_SERVICE_TEMPLATE-SemanticTemplateName
      ).
    endif.
  endmethod.


  method FILL_SERVICE_TASK_DATA.

    FR_SERVICE_TASK->SERVICETASK->* = corresponding #( IS_SRVC_TSK_TMPL mapping SequenceNumber = SqncNr ).
    FR_SERVICE_TASK->SERVICETASK->ServiceUUID = FR_SERVICE->SERVICE->ServiceUUID.

    FO_ACCESS_POOL->SET_CONTEXT(
      IV_SEMANTIC_CONTEXT = /PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-SERVICE
      IR_DATA             = FR_SERVICE
    ).
    FO_ACCESS_POOL->SET_CONTEXT(
      IV_SEMANTIC_CONTEXT = 'EWA_WASTE_ORDER_ITEM' "/PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-???
      IR_DATA             = FR_WASTE_ORDER_ITEM
    ).
    FO_ACCESS_POOL->SET_CONTEXT(
      IV_SEMANTIC_CONTEXT = /PLCE/CL_SEMANTIC_CONSTANTS=>C_SEMANTIC_CONTEXT-SERVICE_TASK
      IR_DATA             = FR_SERVICE_TASK
    ).
    FO_MAPPER->EXECUTE_SEMANTIC_TEMPL_CONFIG(
      IV_PROFILE                = GET_PROFILE( )
      IV_SEMANTIC_TEMPLATE_NAME = IS_SRVC_TSK_TMPL-SemanticTemplateName
    ).


  endmethod.


  method GET_ACTION.
    clear RV_ACTION.
    RV_ACTION = conv #( FR_WASTE_ORDER_ITEM->SERVICE_TYPE ).
*    RV_ACTION = /PLCE/CL_PD_MISC=>GET_ACTION( FR_SERVICE_ORDER_ITEM->PRODUCT ).
*    if RV_ACTION is initial.
*      read table GET_CHARC_VALUES_FOR_SOI( FR_SERVICE_ORDER_ITEM->ServiceOrderItemUuid )-VALUES into data(LS_AVC_VALUES)
*        with key NAME = GET_CHARC_NAME_FOR_SEMANTIC( /PLCE/CL_PD_WCS_CONSTANTS=>C_SEMANTIC_NAME-PD_ACTION ).
*      if SY-SUBRC is initial.
*        RV_ACTION = LS_AVC_VALUES-VALUE.
*      endif.
*    endif.
  endmethod.


  method GET_NEXT_CID.
    FV_CID_NUMBER = FV_CID_NUMBER + 1.
    RV_CID = |CID{ FV_CID_NUMBER }|.
  endmethod.


  method GET_PROFILE.
    RV_PROFILE = FV_PROFILE.
  endmethod.


  method INIT_CID.
    clear FV_CID_NUMBER.
  endmethod.


  method INIT_MESSAGE_LOG.

    create object FO_MSGLOG
      exporting
        IV_OBJECT      = C_MESSAGE_LOG_BASE-LOG_OBJECT
        IV_SUBOBJECT   = C_MESSAGE_LOG_BASE-LOG_SUBOBJECT
        IV_EXTERNAL_ID = conv #( |{ C_MESSAGE_LOG_BASE-LOG_EXTID }'@'{ /PLCE/CL_BASE_MISC=>GET_CURRENT_DATE_TIME( ) }| ).

  endmethod.


  method PROCESS_SERVICES.

    CREATE_SERVICES(
      changing
        CT_SRV_KEY = CT_SRV_KEY
        CT_SRV_MAPPING = CT_SRV_MAPPING
    ).

    UPDATE_SERVICES(
      changing
        CT_SRV_KEY = CT_SRV_KEY
        CT_SRV_MAPPING = CT_SRV_MAPPING
    ).

    COMPOSE_TOURS(
      exporting
        IT_SRV_KEY  = CT_SRV_KEY
        IT_SRV_MAPPING = CT_SRV_MAPPING
    ).

  endmethod.


  method PROCESS_UPDATE.
    types:
      begin of _TATTAKEY,
        ServiceUUID    type /PLCE/PDSERVICE_UUID,
        AttachmentUUID type /PLCE/PDATTACHMENT_UUID,
      end of _TATTAKEY.

    data:
      LSRVKEYS    type /PLCE/PPDSRV_KEY,
      LSRVTSKKEYS type /PLCE/PPDSRVTSK_KEY.


    data:
      LRS_UPD       type table for update /PLCE/R_PDSERVICE,
      LRSUNI_UPD    type table for update /PLCE/R_PDSERVICEEXTUNI,
      LRSWA_UPD     type table for update /PLCE/R_PDSERVICEEXTWR,
      LRSTSK_UPD    type table for update /PLCE/R_PDSERVICETASK,
      LRSTSKUNI_UPD type table for update /PLCE/R_PDSERVICETASKEXTUNI,
      LRSTSKWA_UPD  type table for update /PLCE/R_PDSERVICETASKEXTWR,
      LRS_ATTA_CREA type table for create /PLCE/R_PDSERVICE\_Attachments.
*      LRS_ATTA_CREA type table for create /PLCE/R_PDServiceAttachment.

    data:
      LSRVKEYS_DEL        type /PLCE/PPDSRV_KEY,
      LSRVKEYS_UNI_DEL    type /PLCE/PPDSRV_KEY,
      LSRVKEYS_WA_DEL     type /PLCE/PPDSRV_KEY,
      LSRVTSKKEYS_DEL     type /PLCE/PPDSRVTSK_KEY,
      LSRVTSKKEYS_UNI_DEL type /PLCE/PPDSRVTSK_KEY,
      LSRVTSKKEYS_WA_DEL  type /PLCE/PPDSRVTSK_KEY,
      LSRATTAKEYS_DEL     type standard table of _TATTAKEY.

    types:
      begin of _TPOBJNR,
        POBJNR type C length 30,
      end of _TPOBJNR.

    data:
      LPOBJNRS type standard table of _TPOBJNR.
****  FILL_KEYS
    if LINES( FT_WASTE_ORDER_ITEM_UPD ) is not initial.
      LPOBJNRS = corresponding #( FT_WASTE_ORDER_ITEM_UPD ).

      if LINES( LPOBJNRS ) is not initial.
        select SERVICEUUID from /PLCE/R_PDSERVICE
          for all entries in @LPOBJNRS
          where REFERENCEINTERNALID = @LPOBJNRS-POBJNR
          into table @LSRVKEYS.
      endif.
      if LINES( LSRVKEYS ) is not initial.

        select SERVICETASKUUID from /PLCE/R_PDSERVICETASK
          for all entries in @LSRVKEYS
          where SERVICEUUID = @LSRVKEYS-SERVICE_UUID
          into table @LSRVTSKKEYS.

      endif.

**** READ_ENTITIES

      read entities of /PLCE/R_PDSERVICE
        entity SERVICE
          all fields
          with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
*    with corresponding #( LKEYS mapping from entity )
          result data(LRS)
*      result ( LRS )
          by \_EXTUNIVERSAL all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSUNI)
          by \_EXTWASTE all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSWA)
          by \_SERVICETASK all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSTSK)
          by \_ATTACHMENTS all fields with value #( for LLINE in LSRVKEYS ( %KEY-SERVICEUUID = LLINE-SERVICE_UUID ) )
          result data(LRSATTA).

      read entities of /PLCE/R_PDSERVICE
        entity SERVICETASK
          by \_EXTWASTE all fields with corresponding #( LRSTSK )  "value #( for L in LRSTSK ( %KEY-ServiceTaskUUID = L-%KEY-ServiceTaskUUID ) )
          result data(LRSTSKWA)
          by \_EXTUNIVERSAL all fields with corresponding #( LRSTSK )  "value #( for L in LRSTSK ( %KEY-ServiceTaskUUID = L-%KEY-ServiceTaskUUID ) )
          result data(LRSTSKUNI).


      LRS_UPD = corresponding #( LRS ).
      LRSUNI_UPD = corresponding #( LRSUNI ).
      LRSWA_UPD = corresponding #( LRSWA ).
      LRSTSK_UPD = corresponding #( LRSTSK ).
      LRSTSKUNI_UPD = corresponding #( LRSTSKUNI ).
      LRSTSKWA_UPD = corresponding #( LRSTSKWA ).

**** READ_ENTITIES

**** MAP_DATA

      data:
        LTSRV     type standard table of /PLCE/TPDSRV,
        LSRV_CREA type /PLCE/TPDSRV,
        LSRV_UPD  type /PLCE/TPDSRV.

      LTSRV = corresponding #( LRS_UPD mapping from entity  ).

      loop at LRS_UPD reference into data(LRSUPDREF).

*    LRSUPDREF->

**** Eigentlich kann es nur einen geben!!!!
        loop at FT_SERVICE_TO_CREATE reference into data(LSERVICECREATEREF)
          where SERVICE-REFERENCEINTERNALID = LRSUPDREF->REFERENCEINTERNALID. "#EC CI_SORTSEQ

**** Base entity
          LSRV_CREA = corresponding #( LSERVICECREATEREF->SERVICE mapping from entity ).
          LSRV_UPD = corresponding #( LRSUPDREF->* mapping from entity ).

          clear:
            LSRV_UPD-SERVICE_UUID, LSRV_UPD-SERVICE_ID.

          if LSRV_UPD-DATA <> LSRV_CREA-DATA.
* match!
            LRSUPDREF->%DATA = corresponding #( base ( LRSUPDREF->%DATA ) LSERVICECREATEREF->SERVICE
              except %KEY SERVICEID SERVICESTATUS REFERENCEID REFERENCEINTERNALID PLANNINGSTATUS ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSUPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
            LRSUPDREF->%CONTROL-SERVICEID = IF_ABAP_BEHV=>MK-OFF.
            LRSUPDREF->%CONTROL-SERVICESTATUS = IF_ABAP_BEHV=>MK-OFF.
            LRSUPDREF->%CONTROL-REFERENCEID = IF_ABAP_BEHV=>MK-OFF.
            LRSUPDREF->%CONTROL-REFERENCEINTERNALID = IF_ABAP_BEHV=>MK-OFF.
            LRSUPDREF->%CONTROL-PLANNINGSTATUS = IF_ABAP_BEHV=>MK-OFF.
          endif.

*** EXTUNI
          read table LRSUNI_UPD reference into data(LRSUNI_UPDREF)
            with table key ENTITY components %KEY-SERVICEUUID = LRSUPDREF->%KEY-SERVICEUUID.

          if SY-SUBRC is initial.
            LRSUNI_UPDREF->%DATA = corresponding #( base ( LRSUNI_UPDREF->%DATA ) LSERVICECREATEREF->EXTUNIVERSAL except %KEY ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSUNI_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
          else.
*** new relation
*          insert LSERVICECREATEREF->EXTUNIVERSAL into table ...
          endif.

*** EXTWA
          read table LRSWA_UPD reference into data(LRSWA_UPDREF)
            with table key ENTITY components %KEY-SERVICEUUID = LRSUPDREF->%KEY-SERVICEUUID.

          if SY-SUBRC is initial.
            LRSWA_UPDREF->%DATA = corresponding #( base ( LRSWA_UPDREF->%DATA ) LSERVICECREATEREF->EXTWASTE except %KEY ).
            /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT =  LRSWA_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
*            LRSWA_UPDREF->* = corresponding #( base ( LRSWA_UPDREF->* ) LSERVICECREATEREF->EXTWASTE changing control except SERVICEUUID %KEY ).
          else.
*** new relation
*          insert LSERVICECREATEREF->EXTWASTE into table ...
          endif.

**** Attachments <- always
          if LINES( LSERVICECREATEREF->ATTACHMENTS ) is not initial.
            insert initial line into table LRS_ATTA_CREA assigning field-symbol(<ATTA_CREA>).
            loop at LSERVICECREATEREF->ATTACHMENTS reference into data(LATTAREF).
              insert initial line into table <ATTA_CREA>-%TARGET assigning field-symbol(<ATTA_CREA_LINE>).
              <ATTA_CREA_LINE> = corresponding #( LATTAREF->ATTACHMENT ).
              <ATTA_CREA_LINE>-ServiceUUID = LRSUPDREF->ServiceUUID.
              <ATTA_CREA_LINE>-%CID = GET_NEXT_CID( ).

              /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = ref #( <ATTA_CREA_LINE> ) ).
              <ATTA_CREA_LINE>-%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
            endloop.
            <ATTA_CREA>-ServiceUUID = LRSUPDREF->ServiceUUID.
*            <ATTA_CREA>-%CID_REF = LRSUPDREF->ServiceUUID.
          endif.

***** Tasks

**** upd & delete

***  update/insert
          loop at LSERVICECREATEREF->T_SERVICE_TASK reference into data(LSERVICETASKCREATEREF).

            read table LRSTSK_UPD reference into data(LRSTSK_UPDREF)
              with key
                       SERVICEUUID = LRSUPDREF->SERVICEUUID  " todo --- Fehler nachstellen
                       TASKTYPE = LSERVICETASKCREATEREF->SERVICETASK-TASKTYPE
                       SEQUENCENUMBER = LSERVICETASKCREATEREF->SERVICETASK-SEQUENCENUMBER.

            if SY-SUBRC is initial.

              LRSTSK_UPDREF->%DATA = corresponding #( base ( LRSTSK_UPDREF->%DATA ) LSERVICETASKCREATEREF->SERVICETASK
                except %KEY SERVICEUUID SERVICETASKID TASKTYPE SEQUENCENUMBER SERVICESTATUS PLANNINGSTATUS TOURUUID ).
              /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSTSK_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
              LRSTSK_UPDREF->%CONTROL-SERVICEUUID = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-SERVICETASKID = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-TASKTYPE = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-SEQUENCENUMBER = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-SERVICESTATUS = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-PLANNINGSTATUS = IF_ABAP_BEHV=>MK-OFF.
              LRSTSK_UPDREF->%CONTROL-TOURUUID = IF_ABAP_BEHV=>MK-OFF.

              read table LRSTSKUNI_UPD reference into data(LRSTSKUNI_UPDREF)
                with table key ENTITY components SERVICETASKUUID = LRSTSK_UPDREF->SERVICETASKUUID.

              if SY-SUBRC is initial.
                LRSTSKUNI_UPDREF->%DATA = corresponding #( base ( LRSTSKUNI_UPDREF->%DATA ) LSERVICETASKCREATEREF->EXTUNIVERSAL except %KEY SERVICEUUID ).
                /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSTSKUNI_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
                LRSTSKUNI_UPDREF->%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
              else.
***           insert new UNI Task
              endif.

              read table LRSTSKWA_UPD reference into data(LRSTSKWA_UPDREF)
                with table key ENTITY components SERVICETASKUUID = LRSTSK_UPDREF->SERVICETASKUUID.

              if SY-SUBRC is initial.
                LRSTSKWA_UPDREF->%DATA = corresponding #( base ( LRSTSKWA_UPDREF->%DATA ) LSERVICETASKCREATEREF->EXTWASTE except %KEY SERVICEUUID ).
                /PLCE/CL_BASE_MISC=>SET_MODIFY_CONTROL_FIELDS( IR_STRUCT = LRSTSKWA_UPDREF IV_SET_ALL_FIELDS = ABAP_TRUE ).
                LRSTSKWA_UPDREF->%CONTROL-ServiceUUID = IF_ABAP_BEHV=>MK-OFF.
              else.
***           insert new WA Task
              endif.


            else.
***       alle Task Teile müssen angelegt werden.

            endif.

          endloop.


****  Delete

          if LSERVICETASKCREATEREF->EXTUNIVERSAL is initial.
            read table LRSUNI_UPD reference into LRSUNI_UPDREF
              with table key ENTITY components %KEY = corresponding #( LRSUPDREF->%KEY ).

            if SY-SUBRC is initial.
              if LSERVICECREATEREF->EXTUNIVERSAL is initial.
                insert corresponding #( LRSUNI_UPDREF->%KEY ) into table LSRVKEYS_UNI_DEL.
              endif.
            endif.
          endif.

          if LSERVICETASKCREATEREF->EXTWASTE is initial.
            read table LRSWA_UPD reference into LRSWA_UPDREF
              with table key ENTITY components %KEY = corresponding #( LRSUPDREF->%KEY ).

            if SY-SUBRC is initial.
              if LSERVICECREATEREF->EXTWASTE is initial.
                insert corresponding #( LRSWA_UPDREF->%KEY ) into table LSRVKEYS_WA_DEL.
              endif.
            endif.
          endif.


          loop at LRSTSK_UPD reference into LRSTSK_UPDREF where SERVICEUUID = LRSUPDREF->SERVICEUUID.
            read table LSERVICECREATEREF->T_SERVICE_TASK  reference into LSERVICETASKCREATEREF "transporting no fields
              with key "SERVICETASK-SERVICEUUID = LRSTSK_UPDREF->SERVICEUUID
                       SERVICETASK-TASKTYPE = LRSTSK_UPDREF->TASKTYPE
                       SERVICETASK-SEQUENCENUMBER = LRSTSK_UPDREF->SEQUENCENUMBER.

            if SY-SUBRC is not initial.
              insert corresponding #( LRSTSK_UPDREF->%KEY ) into table LSRVTSKKEYS_DEL.
            else.

              read table LRSTSKUNI_UPD reference into LRSTSKUNI_UPDREF
                with table key ENTITY components %KEY = corresponding #( LRSTSK_UPDREF->%KEY ).

              if SY-SUBRC is initial.
                if LSERVICETASKCREATEREF->EXTUNIVERSAL is initial.
                  insert corresponding #( LRSTSK_UPDREF->%KEY ) into table LSRVTSKKEYS_UNI_DEL.
                endif.
              endif.

              read table LRSTSKWA_UPD reference into LRSTSKWA_UPDREF
                  with table key ENTITY components %KEY = corresponding #( LRSTSK_UPDREF->%KEY ).

              if SY-SUBRC is initial.
                if LSERVICETASKCREATEREF->EXTWASTE is initial.
                  insert corresponding #( LRSTSK_UPDREF->%KEY ) into table LSRVTSKKEYS_WA_DEL.
                endif.
              endif.

            endif.

          endloop.

        endloop.
      endloop.

*** Attachments
      LSRATTAKEYS_DEL = value #( for LATTALINE in LRSATTA where ( CreatedByUpload is not initial )
        ( corresponding #( LATTALINE ) ) ).

**** SAVE
      delete:
        LRS_UPD where %CONTROL is initial,
        LRSUNI_UPD where %CONTROL is initial,
        LRSWA_UPD where %CONTROL is initial,
        LRSTSK_UPD where %CONTROL is initial,
        LRSTSKWA_UPD where %CONTROL is initial,
        LRSTSKUNI_UPD where %CONTROL is initial.


      modify entities of /PLCE/R_PDSERVICE
        entity SERVICE
          update from LRS_UPD
*          update set fields with corresponding #( LRS_UPD ) "set fields with LRS_UPD  ##SETFIELDS_OK
          delete from corresponding #( LSRVKEYS_DEL )
          create by \_Attachments from corresponding #( LRS_ATTA_CREA )
        entity EXTUNIVERSAL
          update from LRSUNI_UPD
*          update set fields with corresponding #( LRSUNI_UPD ) ##SETFIELDS_OK
          delete from corresponding #( LSRVKEYS_UNI_DEL )
        entity EXTWASTE
*          update set fields with corresponding #( LRSWA_UPD ) ##SETFIELDS_OK
          update from LRSWA_UPD
          delete from corresponding #( LSRVKEYS_WA_DEL )
        entity Attachments
          delete from corresponding #( LSRATTAKEYS_DEL )
        entity SERVICETASK
          update from LRSTSK_UPD
*          update set fields with corresponding #( LRSTSK_UPD except SERVICEUUID SEQUENCENUMBER ) ##SETFIELDS_OK
          delete from corresponding #( LSRVTSKKEYS_DEL )
        entity EXTTASKUNIVERSAL
          update from LRSTSKUNI_UPD
*          update set fields with corresponding #( LRSTSKUNI_UPD except SERVICEUUID ) ##SETFIELDS_OK
          delete from corresponding #( LSRVTSKKEYS_UNI_DEL )
        entity EXTTASKWASTE
          update from LRSTSKWA_UPD
*          update set fields with corresponding #( LRSTSKWA_UPD except SERVICEUUID  ) ##SETFIELDS_OK
          delete from corresponding #( LSRVTSKKEYS_WA_DEL )
        mapped data(MAPPED_UPDATE)
        failed data(FAILED_UPDATE)
        reported data(REPORTED_UPDATE).

      if FAILED_UPDATE is initial.
*        modify entities of /PLCE/R_PDSERVICE
*          entity SERVICE
**            update from LRS_UPD
**  *          update set fields with corresponding #( LRS_UPD ) "set fields with LRS_UPD  ##SETFIELDS_OK
**            delete from corresponding #( LSRVKEYS_DEL )
*            create by \_Attachments from corresponding #( LRS_ATTA_CREA )
*          mapped data(MAPPED_UPDATE_ATTA)
*          failed data(FAILED_UPDATE_ATTA)
*          reported data(REPORTED_UPDATE_ATTA).

*        if FAILED_UPDATE_ATTA is initial.
          commit entities
            response of /PLCE/R_PDSERVICE
              failed data(FAILED_COMMIT_SRVC)
              reported data(REPORTED_COMMIT_SRVC).
*        else.
*          rollback entities.                          "#EC CI_ROLLBACK.
*        endif.
      else.
        rollback entities.                            "#EC CI_ROLLBACK.
      endif.

      /PLCE/CL_BASE_MISC=>CHECK_RESPONSE(
        exporting
          IS_RESPONSE = REPORTED_UPDATE
          IR_LOG      = FO_MSGLOG                 " APPLOG
      ).

    endif.

  endmethod.


  method PROCESS_WASTEORDERS.
    clear:
      ET_SRV_KEY,
      FT_SERVICE_TO_CREATE,
      FT_WASTE_ORDER_ITEM_CREA,
      FT_WASTE_ORDER_ITEM_UPD.

    if IO_MSGLOG is not initial.
      FO_MSGLOG = IO_MSGLOG.
    endif.

    SET_PROFILE( IV_PROFILE ).
    INIT_CID( ).
    SET_DATA( IT_WASTEORDER ).

    PROCESS_SERVICES(
      changing
        CT_SRV_KEY = ET_SRV_KEY
        CT_SRV_MAPPING = ET_SRV_MAPPING ).



  endmethod.


  method READ_SERVICE_STATUS_BY_SOIKEYS.
    clear RT_SERVICE_STATUS.
*    data: LT_EXTERNAL_ID type standard table of /PLCE/PDEXTERNAL_ID.
*    loop at FT_SERVICE_ORDER_ITEM_KEY reference into data(LR_SERVICE_ORDER_ITEM).
*      insert conv #( LR_SERVICE_ORDER_ITEM->SERVICEORDER ) into table LT_EXTERNAL_ID.
*    endloop.
*
*    LT_EXTERNAL_ID = value #( for L in FT_SERVICE_ORDER_ITEM_KEY ( conv #( LR_SERVICE_ORDER_ITEM->SERVICEORDER ) ) ).
*
*    if LINES( LT_EXTERNAL_ID ) > 0.
*      select distinct *
*        from /PLCE/R_PDService
**        from /plce/tpdsrv
*        for all entries in @LT_EXTERNAL_ID
*        where ReferenceId = @LT_EXTERNAL_ID-TABLE_LINE
*        into corresponding fields of table @RT_SERVICE_STATUS.
*      if SY-SUBRC is not initial.
*        clear RT_SERVICE_STATUS.
*      endif.
*    endif.
  endmethod.


  method READ_SERVICE_TASK_TEMPLATES.

    clear RT_SERVICE_TASK_TMPL.
    read table FR_PROFILE->T_SERVICE_TYPE reference into data(LR_SERVICE_TYPE) with key SERVICETYPE = IV_SERVICETYPE.
    if SY-SUBRC is not initial.
      insert value #( ServiceType = IV_SERVICETYPE ) into table FR_PROFILE->T_SERVICE_TYPE reference into LR_SERVICE_TYPE.
      data(LV_PROFILE) = GET_PROFILE( ).
      select * from /PLCE/R_PDSERVICETASKTMPL
        where Profile = @LV_PROFILE
          and ServiceType = @IV_SERVICETYPE
        into table @data(LT_PDSERVICETASKTMPL).
      if SY-SUBRC is not initial.
        clear LT_PDSERVICETASKTMPL.
      endif.
      loop at LT_PDSERVICETASKTMPL assigning field-symbol(<LS_PDSERVICETASKTMPL>).
        if not LINE_EXISTS( LR_SERVICE_TYPE->T_SERVICE_TASK_TEMPLATE[ ServiceType = <LS_PDSERVICETASKTMPL>-ServiceType SQNCNR = <LS_PDSERVICETASKTMPL>-SQNCNR PROFILE = <LS_PDSERVICETASKTMPL>-PROFILE ] ).
          insert <LS_PDSERVICETASKTMPL> into table LR_SERVICE_TYPE->T_SERVICE_TASK_TEMPLATE.
        endif.
      endloop.

    endif.
    if LINES( LR_SERVICE_TYPE->T_SERVICE_TASK_TEMPLATE ) <= 0.
      FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
        exporting
          IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
          IV_ID         = '/PLCE/MC_PD_MSG'
          IV_NUMBER     = '109'
          IV_VARIABLE_1 = conv #( IV_SERVICETYPE )
          IV_VARIABLE_2 = conv #( GET_PROFILE( ) )
      ).
    endif.
    RT_SERVICE_TASK_TMPL = LR_SERVICE_TYPE->T_SERVICE_TASK_TEMPLATE.

  endmethod.


  method READ_SERVICE_TEMPLATES.
    clear RT_SERVICE_TMPL.
    read table FR_PROFILE->T_ACTION reference into data(LR_ACTION) with key ACTION = IV_ACTION.
    if SY-SUBRC is not initial.
      insert value #( ACTION = IV_ACTION ) into table FR_PROFILE->T_ACTION reference into LR_ACTION.
      data(LV_PROFILE) = GET_PROFILE( ).
      select * from /PLCE/R_PDServiceTemplate
        where Profile = @LV_PROFILE
          and ACTION = @IV_ACTION
        into table @data(LT_PDSERVICETEMPLATE). "#EC CI_ALL_FIELDS_NEEDED
      if SY-SUBRC is not initial.
        clear LT_PDSERVICETEMPLATE.
      endif.
      loop at LT_PDSERVICETEMPLATE assigning field-symbol(<LS_PDSERVICETEMPLATE>).
*        if not LINE_EXISTS( LR_ACTION->T_SERVICE_TEMPLATE[ ACTION = <LS_PDSERVICETEMPLATE>-ACTION SQNCNR = <LS_PDSERVICETEMPLATE>-SQNCNR PROFILE = <LS_PDSERVICETEMPLATE>-PROFILE ] ).
        if not LINE_EXISTS( LR_ACTION->T_SERVICE_TEMPLATE[ ACTION = <LS_PDSERVICETEMPLATE>-ACTION PROFILE = <LS_PDSERVICETEMPLATE>-PROFILE ] ).
          insert <LS_PDSERVICETEMPLATE> into table LR_ACTION->T_SERVICE_TEMPLATE.
        endif.
      endloop.
    endif.
    if LINES( LR_ACTION->T_SERVICE_TEMPLATE ) <= 0.
      FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
        exporting
          IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
          IV_ID         = '/PLCE/MC_PD_MSG'
          IV_NUMBER     = '108'
          IV_VARIABLE_1 = conv #( GET_PROFILE( ) )
          IV_VARIABLE_2 = conv #( IV_ACTION )
      ).
    endif.
    RT_SERVICE_TMPL = LR_ACTION->T_SERVICE_TEMPLATE.
  endmethod.


  method SAVE_SERVICE.

    check FT_SERVICE_TO_CREATE is not initial.

    data: LT_SERVICE_TO_CREATE         type table for create /PLCE/R_PDSERVICE,
          LT_SERVICE_TASK_TO_CREATE    type table for create /PLCE/R_PDSERVICE\_SERVICETASK,
          LT_SERVICE_UNIVERSAL         type table for create /PLCE/R_PDSERVICE\_EXTUNIVERSAL,
          LT_SERVICE_WASTE             type table for create /PLCE/R_PDSERVICE\_EXTWASTE,
          LT_SERVICE_TASK_TO_CREATE_UN type table for create /PLCE/R_PDSERVICETASK\_EXTUNIVERSAL,
          LT_SERVICE_TASK_TO_CREATE_WA type table for create /PLCE/R_PDSERVICETASK\_EXTWASTE,
          LT_SERVICE_NOTES_TO_CREATE   type table for create /PLCE/R_PDSERVICE\_Notes,
          LT_SERVICE_ATTAS_TO_CREATE   type table for create /PLCE/R_PDSERVICE\_Attachments,
          LV_SERVICETASKCID            type ABP_BEHV_CID,
          LINDEX                       type N length 8.

    loop at FT_SERVICE_TO_CREATE reference into data(LR_SERVICE_TO_CREATE).
      insert value #( %CID = LR_SERVICE_TO_CREATE->CID %DATA = LR_SERVICE_TO_CREATE->SERVICE ) into table LT_SERVICE_TO_CREATE.

      if LR_SERVICE_TO_CREATE->EXTUNIVERSAL is not initial.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_UNIVERSAL assigning field-symbol(<LS_SERVICE_UNIVERSAL>).
        insert value #( %CID = |{ LR_SERVICE_TO_CREATE->CID }_EXTUN| %DATA = LR_SERVICE_TO_CREATE->EXTUNIVERSAL ) into table <LS_SERVICE_UNIVERSAL>-%TARGET.
      endif.
      if LR_SERVICE_TO_CREATE->EXTWASTE is not initial.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_WASTE assigning field-symbol(<LS_SERVICE_WASTE>).
        insert value #( %CID = |{ LR_SERVICE_TO_CREATE->CID }_EXTWA| %DATA = LR_SERVICE_TO_CREATE->EXTWASTE ) into table <LS_SERVICE_WASTE>-%TARGET.
      endif.

*      insert value #( %CID = LR_SERVICE_TO_CREATE->CID %DATA = LR_SERVICE_TO_CREATE->SERVICE ) into table LT_SERVICE_TO_CREATE.

      if LINES( LR_SERVICE_TO_CREATE->T_SERVICE_TASK ) > 0.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_TASK_TO_CREATE assigning field-symbol(<LS_SERVICE_TASK_TO_CREATE>).
        loop at LR_SERVICE_TO_CREATE->T_SERVICE_TASK reference into data(LR_SERVICE_TASK).
          concatenate LR_SERVICE_TO_CREATE->CID '_' LR_SERVICE_TASK->SERVICETASK-SEQUENCENUMBER into LV_SERVICETASKCID.
          insert value #( %CID = LV_SERVICETASKCID %DATA = LR_SERVICE_TASK->SERVICETASK ) into table <LS_SERVICE_TASK_TO_CREATE>-%TARGET.

          if LR_SERVICE_TASK->EXTUNIVERSAL is not initial.
            insert value #( %CID_REF = LV_SERVICETASKCID ) into table LT_SERVICE_TASK_TO_CREATE_UN assigning field-symbol(<LS_SERVICETASK_UNIVERSAL>).
            insert value #( %CID = |{ LV_SERVICETASKCID }_EXTUN| %DATA = LR_SERVICE_TASK->EXTUNIVERSAL ) into table <LS_SERVICETASK_UNIVERSAL>-%TARGET.
          endif.
          if LR_SERVICE_TO_CREATE->EXTWASTE is not initial.
            insert value #( %CID_REF = LV_SERVICETASKCID ) into table LT_SERVICE_TASK_TO_CREATE_WA assigning field-symbol(<LS_SERVICETASK_WASTE>).
            insert value #( %CID = |{ LV_SERVICETASKCID }_EXTWA| %DATA = LR_SERVICE_TASK->EXTWASTE ) into table <LS_SERVICETASK_WASTE>-%TARGET.
          endif.

        endloop.
      endif.
      if LINES( LR_SERVICE_TO_CREATE->T_SERVICE_NOTES ) > 0.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_NOTES_TO_CREATE assigning field-symbol(<LS_SERVICE_NOTES_TO_CREATE>).
        loop at LR_SERVICE_TO_CREATE->T_SERVICE_NOTES reference into data(LR_SERVICE_NOTE).
          LINDEX = LINDEX + 1.
          concatenate LR_SERVICE_TO_CREATE->CID '_NOTE' LINDEX into LV_SERVICETASKCID.
          insert value #( %CID = LV_SERVICETASKCID %DATA = LR_SERVICE_NOTE->SERVICE_NOTE ) into table <LS_SERVICE_NOTES_TO_CREATE>-%TARGET.
        endloop.
      endif.
      if LINES( LR_SERVICE_TO_CREATE->ATTACHMENTS ) > 0.
        insert value #( %CID_REF = LR_SERVICE_TO_CREATE->CID ) into table LT_SERVICE_ATTAS_TO_CREATE assigning field-symbol(<LS_SERVICE_ATTAS_TO_CREATE>).
        loop at LR_SERVICE_TO_CREATE->ATTACHMENTS reference into data(LR_SERVICE_ATTAREF).
          LINDEX = LINDEX + 1.
          concatenate LR_SERVICE_TO_CREATE->CID '_ATTA' LINDEX into LV_SERVICETASKCID.
          insert value #( %CID = LV_SERVICETASKCID %DATA = LR_SERVICE_ATTAREF->ATTACHMENT ) into table <LS_SERVICE_ATTAS_TO_CREATE>-%TARGET.
        endloop.
      endif.
    endloop.

    modify entities of /PLCE/R_PDSERVICE
      entity SERVICE
        create set fields with LT_SERVICE_TO_CREATE  ##SETFIELDS_OK
        create by \_SERVICETASK set fields with LT_SERVICE_TASK_TO_CREATE
*        create by \_ServiceTask ExtUniversal set fields with LT_SERVICE_TASK_TO_CREATE
        create by \_EXTUNIVERSAL set fields with LT_SERVICE_UNIVERSAL
        create by \_EXTWASTE set fields with LT_SERVICE_WASTE
        create by \_Notes set fields with LT_SERVICE_NOTES_TO_CREATE
        create by \_Attachments set fields with LT_SERVICE_ATTAS_TO_CREATE
      entity SERVICETASK
        create by \_EXTUNIVERSAL set fields with LT_SERVICE_TASK_TO_CREATE_UN
        create by \_EXTWASTE set fields with LT_SERVICE_TASK_TO_CREATE_WA
      mapped data(MAPPED_CREATE)
      failed data(FAILED_CREATE)
      reported data(REPORTED_CREATE).


    loop at FAILED_CREATE-SERVICE assigning field-symbol(<FAILED_CREATE_SRVC>).
      FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
        exporting
          IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
          IV_ID         = '/PLCE/MC_PD_MSG'
          IV_NUMBER     = '104'
          IV_VARIABLE_1 = conv #( text-005 )
          IV_VARIABLE_2 = conv #( <FAILED_CREATE_SRVC>-SERVICEUUID )
      ).
    endloop.

    commit entities
      response of /PLCE/R_PDSERVICE
        failed data(FAILED_COMMIT_SRVC)
        reported data(REPORTED_COMMIT_SRVC).

    if FAILED_COMMIT_SRVC is not initial or sy-subrc is not initial.
      loop at FAILED_COMMIT_SRVC-SERVICE assigning field-symbol(<FAILED_COMMIT_SRVC>).
        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_MESSAGE(
          exporting
            IV_SEVERITY   = FO_MSGLOG->/PLCE/IF_PROTOCOL~C_SEVERITY-ERROR
            IV_ID         = '/PLCE/MC_PD_MSG'
            IV_NUMBER     = '106'
            IV_VARIABLE_1 = conv #( text-005 )
            IV_VARIABLE_2 = conv #( <FAILED_COMMIT_SRVC>-SERVICEUUID )
        ).
      endloop.
      rollback entities.                               "#EC CI_ROLLBACK
*      clear ET_SERVICE_KEY.
    else.
      if ET_SERVICE_KEY is requested or ET_SRV_MAPPING is requested.

        ET_SERVICE_KEY = corresponding #( MAPPED_CREATE-SERVICE ).
        loop at MAPPED_CREATE-SERVICE assigning field-symbol(<MAPPED_CREATE_SRVC>).
          ET_SERVICE_KEY = value #( base ET_SERVICE_KEY ( SERVICE_UUID = <MAPPED_CREATE_SRVC>-%KEY-SERVICEUUID ) ).
        endloop.

      endif.

      if ET_SRV_MAPPING is requested.

        read entities of /PLCE/R_PDSERVICE
          entity SERVICE
          fields ( SERVICEUUID REFERENCEINTERNALID )
*          with value #( for LLINE in MAPPED_CREATE-SERVICE ( %KEY-SERVICEUUID = LLINE-%KEY-SERVICEUUID ) )
          with corresponding #( MAPPED_CREATE-SERVICE )
          result data(LRS).

        ET_SRV_MAPPING = corresponding #( LRS mapping POBJNR = ReferenceInternalId SERVICE_UUID = ServiceUUID ).

      endif.
    endif.

  endmethod.


  method SET_DATA.
    types:
      begin of _TKEY,
        POBJNR type C length 30,
      end of _TKEY,
      begin of _TEKY_READ,
        POBJNR          type /PLCE/J_OBJNR,
        SERVICE_UUID    type /PLCE/PDSERVICE_UUID,
        PLANNING_STATUS type /PLCE/PDPLANNING_STATUS,
        SERVICESTATUS   type /PLCE/PDSERVICE_STATUS,
        WORKSTATUS      type N length 1,
        TOURSTATUS       type /PLCE/PDTOUR_STATUS,
      end of _TEKY_READ.


    data:
      LKEYS      type standard table of _TKEY with non-unique sorted key SS_POBJNR components POBJNR,
      LKEYS_READ type standard table of _TEKY_READ "/PLCE/CL_PD_IMP_WASTE_ORDERS=>TS_WDOI_SRV_MAPPING
        with non-unique sorted key SS_POBJNR components POBJNR.

    if LINES( IT_WASTE_ORDER ) is not initial.

      LKEYS = corresponding #( IT_WASTE_ORDER ).

      if LINES( LKEYS ) is not initial.

        select REFERENCEINTERNALID, SRV~SERVICEUUID, PLANNINGSTATUS, SRV~SERVICESTATUS, LOOKUP~WORKSTATUS, TOUR~TOURSTATUS
          from /PLCE/R_PDSERVICE as SRV
            inner join /PLCE/R_PDService_L as LOOKUP
            on SRV~ServiceUUID = LOOKUP~ServiceUUID
          left outer join /PLCE/P_PDTourAssignment as TASS
            on TASS~ServiceUUID = SRV~ServiceUUID and TASS~Removed = @SPACE
          left outer join /PLCE/R_PDTour as Tour
            on Tour~TourUUID = TASS~TourUUID
          for all entries in @LKEYS
          where REFERENCEINTERNALID = @LKEYS-POBJNR
          into table @LKEYS_READ.


        if SY-SUBRC is initial and LINES( LKEYS_READ ) is not initial.

          FT_WASTE_ORDER_ITEM_CREA = corresponding #( filter #( IT_WASTE_ORDER except in LKEYS_READ using key SS_POBJNR where POBJNR = POBJNR ) ).

          loop at LKEYS_READ reference into data(LKEYREADREF)
            where not ( ( PLANNING_STATUS is initial or PLANNING_STATUS = 'X' )
              and ( WORKSTATUS is initial or WORKSTATUS = '1' )
              and ( SERVICESTATUS is initial or SERVICESTATUS = '01' )
              and ( TOURSTATUS is initial or TOURSTATUS = '01' ) ).
*          loop at LKEYS_READ reference into data(LKEYREADREF) where PLANNING_STATUS is not initial.
*            and WORKSTATUS > 1.  <--- todo Update -> Status and ServiceId are deleted!!!!!!!
***         todo Message with Ordernr Laufnr and POBJNR that service can't be updated
            delete LKEYS_READ.
          endloop.

          FT_WASTE_ORDER_ITEM_UPD = corresponding #( filter #( IT_WASTE_ORDER in LKEYS_READ using key SS_POBJNR where POBJNR = POBJNR ) ).

        else.
          FT_WASTE_ORDER_ITEM_CREA = IT_WASTE_ORDER.
        endif.
      endif.
    endif.

  endmethod.


  method SET_PROFILE.
    FV_PROFILE = IV_PROFILE.
    read table FT_PROFILE reference into FR_PROFILE with key PROFILE = FV_PROFILE.
    if SY-SUBRC is not initial.
      insert value #( PROFILE = FV_PROFILE ) into table FT_PROFILE reference into FR_PROFILE.
    endif.
    FO_ACCESS_POOL = new /PLCE/CL_SEM_ACS_POOL( IV_PROFILE = FV_PROFILE ).
    FO_MAPPER = /PLCE/CL_SEMANTIC_MAPPER=>GET_MAPPER( FO_ACCESS_POOL ).
  endmethod.


  method SET_SERVICE_ORDER_ITEM_KEYS.
*    FT_SERVICE_ORDER_ITEM_KEY        = IT_SERVICE_ORDER_ITEM_KEY.
*    data(LT_SERVICE_STATUS) = READ_SERVICE_STATUS_BY_SOIKEYS( ).
*    loop at FT_SERVICE_ORDER_ITEM_KEY reference into data(LR_SERVICE_ORDER_ITEM).
*      read table LT_SERVICE_STATUS reference into data(LR_SERVICE_STATUS) with table key SOI components REFERENCEID = LR_SERVICE_ORDER_ITEM->SERVICEORDER REFERENCEINTERNALID = LR_SERVICE_ORDER_ITEM->SERVICEORDERITEM.
*      if SY-SUBRC is not initial.
*        insert LR_SERVICE_ORDER_ITEM->* into table FT_SERVICE_ORDER_ITEM_KEY_CREA.
*      else.
*        if LR_SERVICE_STATUS->PLANNINGSTATUS = /PLCE/IF_PD_CONSTANTS=>C_PD_PLANNING_STATUS-UNPLANNED.
*          insert LR_SERVICE_ORDER_ITEM->* into table FT_SERVICE_ORDER_ITEM_KEY_UPD.
*        endif.
*      endif.
*    endloop.
  endmethod.


  method UPDATE_SERVICES.

    try.

        FILL_SERVICES( IT_WASTE_ORDER_ITEM_DATA = FT_WASTE_ORDER_ITEM_UPD ).

        PROCESS_UPDATE( ).

      catch /PLCE/CX_BASEEXCEPTION into data(LBASEEX).
        FO_MSGLOG->/PLCE/IF_PROTOCOL~REPORT_EXCEPTION(
          exporting
*           IV_SEVERITY  =
            IV_EXCEPTION = LBASEEX
        ).
    endtry.

  endmethod.
ENDCLASS.


class /PLCE/CL_PD_IMP_WASTE_ORDERS definition
  public
  create public .

  public section.

*    types:
*      begin of TS_ADDITIONAL_SERVICE,
*        MATNR      type /PLCE/PDTRANSPORT_PRODUCT,
*        MATNR_UNIT type /PLCE/PDQUANTITY_UNIT,
*      end of TS_ADDITIONAL_SERVICE,
*      begin of TS_NOTE,
*        NOTE     type C length 5,
*        NOTE_TXT type C length 60,
*      end of TS_NOTE,
*      begin of TS_ATTACHMENT,
*        ATTACHMENT type /PLCE/PDATTACHMENT,
*        FILENAME   type /PLCE/FILENAME,
*        MIMETYPE   type /PLCE/PDMIMETYPE,
*      end of TS_ATTACHMENT,
*      TP_ADDITIONAL_SERVICE type standard table of TS_ADDITIONAL_SERVICE with default key,
*      TP_NOTE               type standard table of TS_NOTE with default key,
*      TP_ATTACHMENT         type standard table of TS_ATTACHMENT with default key.
*    types:
*      begin of TS_WASTEORDER,
*        POBJNR                 type C length 22,
*        ORDERNR                type C length 20,
*        ORDER_LAUFNR           type N length 4,
*        WDOITEMID              type C length 20,
*        PRODUCT_AREA           type C length 2,
*        SERVICE_TYPE           type C length 2,
*        SERVICE_CAT            type C length 2,
*        SERVICE_FREQ           type /PLCE/PDSERVICE_FREQ,
*        SERVLOC                type C length 30,
*        WASTE_TYPE             type C length 40,
**        WASTE_WEIGHT           type QUAN, " length  10  decimals  3,
*        WASTE_WEIGHT           type P length 10  decimals  3,
*        WEIGHT_UNIT            type /PLCE/PDTRANSP_PROD_QTY_UNT,
*        ORDER_DATE             type /PLCE/DATE,
*        KUNNR                  type C length 10,
*        WDPLANTNR              type C length 30,
*        ROUTE                  type C length 10,
*        ROUTE_SEQUENCE         type C length 4,
*        IS_ROUTE_SERVICE_BASED type C length 1,
*        BEH_TYPE               type C length 40,
*        BEH_ANZAHL             type /PLCE/AMOUNT_3,
*        BEH_TYPE_NEW           type C length 40,
*        BEH_ANZAHL_NEW         type /PLCE/AMOUNT_3,
*        TIDNR                  type C length 25,
*        TIDNR_NEW              type C length 25,
*        CUSTOMER_INFO          type C length 80,
*        ORDERTXT               type C length 255,
*        TIMEFRAME              type /PLCE/PDSERVICE_WINDOW, "TIMEFRAME, " CHAR  2
*        START_TIME             type /PLCE/PDSERVICE_WINDOW_START, " TIMS  6
*        END_TIME               type /PLCE/PDSERVICE_WINDOW_END, "  TIMS  6
*        EWC_CODE               type C length 20,
*        NOTES                  type TP_NOTE,
*        ADDITIONAL_SERVICES    type TP_ADDITIONAL_SERVICE,
*        ATTACHMENTS            type TP_ATTACHMENT,
*      end of TS_WASTEORDER .
*    types:
*      TP_WASTEORDER type sorted table of TS_WASTEORDER with unique key POBJNR
*          with non-unique sorted key SS_SERVICE_TYPE components SERVICE_TYPE .
    types:
      TP_WASTEORDER type /PLCE/PWR_WASTEORDERITEM_IMP,
      TS_WASTEORDER type /PLCE/SWR_WASTEORDERITEM_IMP.
    types:
      begin of TS_WDOI_SRV_MAPPING,
        POBJNR       type C length 22,
        SERVICE_UUID type /PLCE/PDSERVICE_UUID,
      end of TS_WDOI_SRV_MAPPING .
    types:
      TP_WDOI_SRV_MAPPING type standard table of TS_WDOI_SRV_MAPPING .

    methods CONSTRUCTOR
      importing
        !IO_MSGLOG type ref to /PLCE/CL_APPLLOG_HELPER optional
      raising
        /PLCE/CX_BASEEXCEPTION .
    methods IMPORT_WASTE_ORDERS
      importing
        !IT_WASTE_ORDER type TP_WASTEORDER
      exporting
        !ET_MAPPING     type TP_WDOI_SRV_MAPPING
        !ET_BAPIRETTAB  type BAPIRETTAB
      raising
        /PLCE/CX_BASEEXCEPTION .
protected section.

  types:
    begin of TS_WASTEORDER_PROFILE,
      PROFILE type /PLCE/PROFILE,
      WASTEORDERS type TP_WASTEORDER,
    end of TS_WASTEORDER_PROFILE .
  types:
    TP_WASTEORDER_PROFILE type standard table of TS_WASTEORDER_PROFILE .

  data FO_MSGLOG type ref to /PLCE/CL_APPLLOG_HELPER .

  methods CONVERT_PROFILE_DATA
    importing
      !IT_WASTE_ORDER type TP_WASTEORDER
    exporting
      !ET_PROFILE_DATA type TP_WASTEORDER_PROFILE
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods PROCESS_SERVICES
    importing
      !IT_PROFILE_DATA type TP_WASTEORDER_PROFILE
    exporting
      !ET_SERVICE_KEY type /PLCE/PPDSRV_KEY
      !ET_SRV_MAPPING type TP_WDOI_SRV_MAPPING
    raising
      /PLCE/CX_BASEEXCEPTION .
  methods INIT_MESSAGE_LOG
    importing
      !IO_MSGLOG type ref to /PLCE/CL_APPLLOG_HELPER optional
    raising
      /PLCE/CX_BASEEXCEPTION .
private section.
ENDCLASS.



CLASS /PLCE/CL_PD_IMP_WASTE_ORDERS IMPLEMENTATION.


  method CONSTRUCTOR.
    INIT_MESSAGE_LOG( IO_MSGLOG ).
  endmethod.


  method CONVERT_PROFILE_DATA.

    types:
      begin of _TACTION,
        ACTION type /PLCE/PDACTION,
      end of _TACTION.

    data:
      LACTIONS   type standard table of _TACTION,
      LACTIONREF type ref to _TACTION,
      LCOUNT     type I,
      LDATAREF   type ref to TS_WASTEORDER_PROFILE.

    LACTIONS = corresponding #( IT_WASTE_ORDER mapping ACTION = SERVICE_TYPE ).
    sort LACTIONS.
    delete adjacent duplicates from LACTIONS.
    delete LACTIONS where ACTION is initial.

    select distinct ( PROFILE ), TEMPL~ACTION from /PLCE/R_PDSERVICETEMPLATE as TEMPL
      inner join @LACTIONS as A on TEMPL~ACTION = A~ACTION ##ITAB_KEY_IN_SELECT
      order by Profile
*      GROUP BY Profile
      into table @data(LACTIONPROFILES).

****  check if Action is unique used in profiles (Waste specific)

    loop at LACTIONS reference into LACTIONREF.
      clear LCOUNT.
      loop at LACTIONPROFILES transporting no fields where ACTION = LACTIONREF->ACTION.
        LCOUNT = LCOUNT + 1.
      endloop.
      if LCOUNT > 1.
        raise exception type /PLCE/CX_BASEEXCEPTION
          message id '/PLCE/MC_PD_MSG' number 297 with LACTIONREF->ACTION ##NUMBER_OK.
      endif.
    endloop.

**** ATC
*    loop at LACTIONPROFILES reference into data(LACTIONPROFILEREF).
*      at new PROFILE.
*        if LDATAREF is initial or LINES( LDATAREF->WASTEORDERS ) is not initial.
*          insert initial line into table ET_PROFILE_DATA reference into LDATAREF.
*        endif.
*        LDATAREF->PROFILE = LACTIONPROFILEREF->PROFILE.
*      endat.
*
*      LDATAREF->WASTEORDERS = corresponding #( base ( LDATAREF->WASTEORDERS )  filter #( IT_WASTE_ORDER using key SS_SERVICE_TYPE where SERVICE_TYPE = conv #( LACTIONPROFILEREF->ACTION ) ) ).
*
*    endloop.

    loop at LACTIONPROFILES reference into data(LACTIONPROFILEREF) group by LACTIONPROFILEREF->PROFILE reference into data(LPROFILEREF).

      insert initial line into table ET_PROFILE_DATA reference into LDATAREF.
      LDATAREF->PROFILE = LPROFILEREF->*.

      loop at group LPROFILEREF reference into data(LGROUPREF).
        LDATAREF->WASTEORDERS = corresponding #( base ( LDATAREF->WASTEORDERS )  filter #( IT_WASTE_ORDER using key SS_SERVICE_TYPE where SERVICE_TYPE = conv #( LGROUPREF->Action ) ) ).
      endloop.
    endloop.


  endmethod.


  method IMPORT_WASTE_ORDERS.

    data:
      LPROFILEDATAS type TP_WASTEORDER_PROFILE,
      LPROFILEREF   type ref to TS_WASTEORDER_PROFILE.

*    try.

        CONVERT_PROFILE_DATA( exporting IT_WASTE_ORDER = IT_WASTE_ORDER
          importing ET_PROFILE_DATA = LPROFILEDATAS ).


        PROCESS_SERVICES(
          exporting
            IT_PROFILE_DATA = LPROFILEDATAS
          importing
*            ET_SERVICE_KEY  =                  " Table type for /PLCE/SPDSRV_KEY
            ET_SRV_MAPPING  = ET_MAPPING
        ).

      FO_MSGLOG->READ_INTO_BAPIRET(
        importing
          ET_BAPIRETTAB = ET_BAPIRETTAB
      ).

  endmethod.


  method INIT_MESSAGE_LOG.
    if IO_MSGLOG is not initial.
      FO_MSGLOG = IO_MSGLOG.
    else.
      create object FO_MSGLOG
        exporting
          IV_OBJECT      = '/PLCE/PD'"C_MESSAGE_LOG_BASE-LOG_OBJECT
          IV_SUBOBJECT   = '/PLCE/PD_SO_TO_SRVC' "C_MESSAGE_LOG_BASE-LOG_SUBOBJECT
          IV_EXTERNAL_ID = conv #( |WasteOrderToService @'{ /PLCE/CL_BASE_MISC=>GET_CURRENT_DATE_TIME( ) }| ) ##NO_TEXT.
    endif.
  endmethod.


  method PROCESS_SERVICES.

*** todo badi anlegen.. & IMPLS
*    loop at T_IMPORT_KEYS assigning field-symbol(<LS_IMPORT_KEYS>).
*      clear LT_SERVICE.
*      get badi LBADI_SO_SRVC
*        filters
*          PROFILE = <LS_IMPORT_KEYS>-PROFILE.
*      if LBADI_SO_SRVC is not initial.
*        call badi LBADI_SO_SRVC->PROCESS_SERVICES
*          exporting
*            IV_PROFILE  = <LS_IMPORT_KEYS>-PROFILE
*            IT_SOI_KEY  = <LS_IMPORT_KEYS>-T_SERVICE_ORDER_ITEM
*            IO_MSGLOG   = FO_MSGLOG
*          changing
*            CT_SRVC_KEY = LT_SERVICE.
*      endif.
*    endloop.

    data:
      LPROFILEREF type ref to TS_WASTEORDER_PROFILE,
      LOBJ type ref to /PLCE/CL_PD_PROC_WASTE_ORDERS,
      LSRVKEYS TYPE /PLCE/PPDSRV_KEY,
      LMAPPINGKEYS type TP_WDOI_SRV_MAPPING.

    create object LOBJ exporting IO_MSGLOG = FO_MSGLOG.

    loop at IT_PROFILE_DATA reference into LPROFILEREF.
      clear:
        LSRVKEYS,
        LMAPPINGKEYS.

      LOBJ->PROCESS_WASTEORDERS(
        exporting
          IV_PROFILE     = LPROFILEREF->PROFILE
          IT_WASTEORDER  = LPROFILEREF->WASTEORDERS
*          IO_MSGLOG      =
        importing
          ET_SRV_MAPPING = LMAPPINGKEYS
          ET_SRV_KEY    = LSRVKEYS
      ).
      insert lines of LSRVKEYS into table ET_SERVICE_KEY.
      insert lines of LMAPPINGKEYS into table ET_SRV_MAPPING.

    endloop.

  endmethod.
ENDCLASS.


<img width="1324" height="830" alt="image" src="https://github.com/user-attachments/assets/2cc9fa67-2912-41d9-970f-35fc38e0dc89" />


