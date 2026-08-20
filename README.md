  method IMPORT_EXT_DATA.
    data:
      LOBJ            type ref to ZCL_WR_PD_IMP_WASTE_ORDERS,
      LBAPIRETTAB     type BAPIRETTAB,
      LS_MESSAGE_DATA type BAL_S_MSG,
      LBAPIRET        type BAPIRET2.



    try.
        CLEAR( ).
*        FS_PRINT_DATA = IS_PRINT_DATA.
*        CONVERT_PRINTDATA( IS_PRINT_DATA = IS_PRINT_DATA ).
        FT_EXTERNAL_DATA = corresponding #( IT_WDOI_DATA ).

****  todo somthing lookup

        create object LOBJ.
*new ZCL_PD_IMPORT_WASTEORDERS( )->IMPORT_WASTE_ORDERS( IT_WASTE_ORDER = LDATAS ).
        LOBJ->IMPORT_WASTE_ORDERS( exporting IT_WASTE_ORDER = corresponding #( FT_EXTERNAL_DATA ) importing ET_BAPIRETTAB = ET_BAPIRET ).

      catch /PLCP/CX_BASE into data(LO_EX).
        LOG_EXCEPTION( IO_EXCEPTION = LO_EX IV_MESSAGE_TYPE = C_MESSAGE_TYPE-ERROR ).
        LBAPIRET = LO_EX->GET_AS_BAPIRET( ).
        insert LBAPIRET into table ET_BAPIRET.


      catch /PLCE/CX_BASEEXCEPTION into data(LOE_EX).
        LOG_EXCEPTION( IO_EXCEPTION = LOE_EX IV_MESSAGE_TYPE = C_MESSAGE_TYPE-ERROR ).
        LBAPIRET = LOE_EX->GET_AS_BAPIRET( ).
        insert LBAPIRET into table ET_BAPIRET.
    endtry.


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
  
  
