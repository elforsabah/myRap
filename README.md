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



<img width="1695" height="732" alt="image" src="https://github.com/user-attachments/assets/b59b37a4-1476-4453-ab52-18fabf14e02a" />
