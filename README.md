class lhc_PDFunctionalLocation definition inheriting from cl_abap_behavior_handler.
  private section.

    methods get_global_authorizations for global authorization
      importing request requested_authorizations for PDFunctionalLocation result result.
    methods get_instance_features for instance features
      importing keys request requested_features for PDFunctionalLocation result result.

    methods geoCode for modify
      importing keys for action PDFunctionalLocation~geoCode result result.
    methods precheck_geoCode for precheck
      importing keys for action PDFunctionalLocation~geoCode.

    methods geocodemanual for modify
      importing keys for action pdfunctionallocation~geocodemanual result result.
    methods geocodeall for modify
      importing keys for action pdfunctionallocation~geocodeall result result.
*    methods createfunctionallocation for modify
*      importing keys for action pdfunctionallocation~createfunctionallocation.

endclass.

class lhc_PDFunctionalLocation implementation.

  method get_global_authorizations.
    authority-check object '/PLCE/FLOC' id 'ACTVT' field '02'.  " change
    data(is_authorized) = cond #( when sy-subrc = 0 then if_abap_behv=>auth-allowed
                                  else if_abap_behv=>auth-unauthorized ).
    result-%update      = is_authorized.
    result-%action-Edit = is_authorized.

    authority-check object '/PLCE/FLOC' id 'ACTVT' field '01'.  " add or create
    is_authorized = cond #( when sy-subrc = 0 then if_abap_behv=>auth-allowed
                            else if_abap_behv=>auth-unauthorized ).
    result-%create      = is_authorized.

    authority-check object '/PLCE/FLOC' id 'ACTVT' field '06'.  " delete
    is_authorized = cond #( when sy-subrc = 0 then if_abap_behv=>auth-allowed
                            else if_abap_behv=>auth-unauthorized ).
    result-%delete      = is_authorized.

    authority-check object '/PLCE/FLOC' id 'ACTVT' field '16'.  " execute
    is_authorized = cond #( when sy-subrc = 0 then if_abap_behv=>auth-allowed
                            else if_abap_behv=>auth-unauthorized ).
    result-%action-geoCode = is_authorized.
    result-%action-geoCodeManual = is_authorized.
    result-%action-geoCodeAll = is_authorized.
  endmethod.

  method get_instance_features.
  endmethod.

  method geoCode.
    data:
      LO_GEOCODER     type ref to /PLCE/IF_GEOCODE,
      LO_GEOCODER_LOG type ref to /PLCE/IF_GEOLOG,
      LGEOCODE_CLASS  type STRING,
      LT_REQUEST      type /PLCE/IF_GEOCODE=>GeoRequests,
      LT_RESULT       type /PLCE/IF_GEOCODE=>GEORESULTS,
      FUNCLOC_UPDATE  type table for update /PLCE/R_PDFunctionalLocation.

    select single * from /PLCE/R_RemoteConfiguration where RemoteConfigurationType =  @/PLCE/IF_PD_CONSTANTS=>C_REMOTE_CONFIG_TYPE-GEOCODING and IsActive = @ABAP_TRUE
    into @data(LCONFIG).

    LGEOCODE_CLASS = LCONFIG-CommunicationClassName.
    create object LO_GEOCODER type (LGEOCODE_CLASS).
    LO_GEOCODER_LOG ?= LO_GEOCODER.

    LO_GEOCODER->INIT( RFCDESTINATION          = LCONFIG-RFCDestination
                       COMMUNICATIONSCENARIOID = LCONFIG-CommunicationScenarioId
                       COMMUNICATIONSERVICEID  = LCONFIG-CommunicationServiceId
                       COMMUNICATIONSYSTEMID   = LCONFIG-CommunicationSystemId ).

    read entities of /PLCE/R_PDFunctionalLocation in local mode
      entity PDFunctionalLocation
*       fields ( FunctionalLocation AddressId ) with corresponding #( keys )
       fields ( FunctionalLocation ) with corresponding #( KEYS )
      result data(LT_FUNCLOC).

***********new
    select AccAss~FunctionalLocation, AccAss~AddressId
      from I_FunctionalLocation as FuncLoc
      join I_LocationAccountAssignment with privileged access as AccAss
        on FuncLoc~MaintObjectLocAcctAssgmtNmbr = AccAss~MaintObjectLocAcctAssgmtNmbr
      join @LT_FUNCLOC as LKEYS
        on LKEYS~FunctionalLocation = FuncLoc~FunctionalLocation
      into table @data(LT_FUNCLOC_ADDRESS).
**********

*    loop at lt_funcloc assigning field-symbol(<ls_funcloc>).
    loop at LT_FUNCLOC_ADDRESS assigning field-symbol(<LS_FUNCLOC>).
      if <LS_FUNCLOC>-AddressID is not initial.
        append initial line to LT_REQUEST reference into data(LREQUEST).
        LREQUEST->KEY = <LS_FUNCLOC>-FunctionalLocation.
        select single * from I_AddrOrgNamePostalAddress with privileged access
        where AddressID = @<LS_FUNCLOC>-AddressID
          and AddressRepresentationCode is initial
          into @LREQUEST->ADDRESS.            "#EC CI_ALL_FIELDS_NEEDED
      else.
        assign LT_FUNCLOC[ key ENTITY FunctionalLocation = <LS_FUNCLOC>-FunctionalLocation ] to field-symbol(<LS_KEY>).
        if <LS_KEY> is assigned.
*          append value #( %key      = <ls_funcloc>-%key ) to failed-pdfunctionallocation.
*          append value #( %key      = <ls_funcloc>-%key
          append value #( %KEY      = <LS_KEY>-%KEY ) to FAILED-PDFUNCTIONALLOCATION.
          append value #( %KEY      = <LS_KEY>-%KEY
                          %MSG        = new /PLCE/CX_PD_EXCEPTION(
                                            SEVERITY = IF_ABAP_BEHV_MESSAGE=>SEVERITY-ERROR
                                            TEXTID = /PLCE/CX_PD_EXCEPTION=>FUNCLOC_NO_ADDRESS
                                            MSGV1 = CONDENSE( | { <LS_FUNCLOC>-FunctionalLocation } | )
                                        )
                          %CREATE = IF_ABAP_BEHV=>MK-ON
          ) to REPORTED-PDFUNCTIONALLOCATION.
        endif.
      endif.
    endloop.

    LO_GEOCODER->GEOCODE( exporting PAR_ADDRESSES = LT_REQUEST importing PAR_RESULTS = LT_RESULT ).
    loop at LT_RESULT reference into data(LRESULT).
      if LRESULT->SUCCESS = ABAP_TRUE.
        append initial line to FUNCLOC_UPDATE assigning field-symbol(<FUNCLOC_UPDATE>).
        <FUNCLOC_UPDATE>-FunctionalLocation = LRESULT->KEY.
        <FUNCLOC_UPDATE>-Latitude = LRESULT->LATITUDE.
        <FUNCLOC_UPDATE>-Longitude = LRESULT->LONGITUDE.
        get time stamp field <FUNCLOC_UPDATE>-LastGeocode.
        <FUNCLOC_UPDATE>-IsManualGeocode = ABAP_FALSE.
        <funcloc_update>-GeocodePrecision = lresult->geocodeprecision.
      else.
        data(LERROR_EX) = new /PLCE/CX_BASEEXCEPTION(
          SEVERITY  = IF_ABAP_BEHV_MESSAGE=>SEVERITY-ERROR
          TEXTID    = /PLCE/CX_BASEEXCEPTION=>CS_FREE_TEXT
          FREE_TEXT = CONDENSE( | { LRESULT->ERROR } | )
        ).

        append value #( %KEY-FUNCTIONALLOCATION      = conv /PLCE/MDFUNCTIONAL_LOCATION( LRESULT->KEY ) ) to FAILED-PDFUNCTIONALLOCATION.
        append value #( %KEY-FUNCTIONALLOCATION      = conv /PLCE/MDFUNCTIONAL_LOCATION( LRESULT->KEY )
                        %MSG        = LERROR_EX
                        %CREATE = IF_ABAP_BEHV=>MK-ON
        ) to REPORTED-PDFUNCTIONALLOCATION.
        if LO_GEOCODER_LOG is not initial.
          LO_GEOCODER_LOG->ADDLOGEXCEPTION( PAR_EXCEPTION = LERROR_EX PAR_EXTERNALID = |{ LRESULT->KEY }| ).
        endif.
      endif.
    endloop.
    if LO_GEOCODER_LOG is not initial.
      LO_GEOCODER_LOG->WRITEMESSAGELOG( ).
    endif.

    modify entities of /PLCE/R_PDFunctionalLocation in local mode
     entity PDFunctionalLocation
     update fields ( Latitude Longitude LastGeocode IsManualGeocode GeocodePrecision ) with corresponding #( FUNCLOC_UPDATE )
       failed data(FAILED_PDFUNCTIONALLOCATION)
       reported data(REPORTED_PDFUNCTIONALLOCATION).

    if failed_pdfunctionallocation is not initial.
      failed = corresponding #( failed_pdfunctionallocation ).
      reported = corresponding #( reported_pdfunctionallocation ).
    endif.

    RESULT = corresponding #( LT_FUNCLOC ).
  endmethod.

  method precheck_geoCode.
    select single * from /PLCE/R_RemoteConfiguration where RemoteConfigurationType = @/PLCE/IF_PD_CONSTANTS=>C_REMOTE_CONFIG_TYPE-GEOCODING and IsActive = @ABAP_TRUE
    into @data(LCONFIG).

    if LCONFIG is initial or LCONFIG-CommunicationClassName is initial.
      FAILED-PDFUNCTIONALLOCATION = value #( for K in KEYS  ( %CID = K-%CID_REF ) ).
      REPORTED-PDFUNCTIONALLOCATION = value #( for K in KEYS
          (
            %CID = K-%CID_REF
            %ACTION-geoCode = IF_ABAP_BEHV=>MK-ON
            %MSG = new /PLCE/CX_PD_EXCEPTION(
                                              SEVERITY = IF_ABAP_BEHV_MESSAGE=>SEVERITY-ERROR
                                              TEXTID = /PLCE/CX_PD_EXCEPTION=>GEOCODE_CFG_MISSING
                                            )
          )
        ).
      return.
    else.
      if /PLCE/CL_BASE_MISC=>IS_ONPREMISE( ) is not initial.
        if LCONFIG-RFCDestination is initial.
          FAILED-PDFUNCTIONALLOCATION = value #( for K in KEYS  ( %CID = K-%CID_REF ) ).
          REPORTED-PDFUNCTIONALLOCATION = value #( for K in KEYS
              (
                %CID = K-%CID_REF
                %ACTION-geoCode = IF_ABAP_BEHV=>MK-ON
                %MSG = new /PLCE/CX_PD_EXCEPTION(
                                                  SEVERITY = IF_ABAP_BEHV_MESSAGE=>SEVERITY-ERROR
                                                  TEXTID = /PLCE/CX_PD_EXCEPTION=>GEOCODE_RFC_MISSING
                                                )
              )
            ).
          return.
        endif.
      else.
        if LCONFIG-CommunicationScenarioId is initial and LCONFIG-CommunicationServiceId is initial and LCONFIG-CommunicationSystemId is initial.
          FAILED-PDFUNCTIONALLOCATION = value #( for K in KEYS  ( %CID = K-%CID_REF ) ).
          REPORTED-PDFUNCTIONALLOCATION = value #( for K in KEYS
              (
                %CID = K-%CID_REF
                %ACTION-geoCode = IF_ABAP_BEHV=>MK-ON
                %MSG = new /PLCE/CX_PD_EXCEPTION(
                                                  SEVERITY = IF_ABAP_BEHV_MESSAGE=>SEVERITY-ERROR
                                                  TEXTID = /PLCE/CX_PD_EXCEPTION=>GEOCODE_CLOUD_CFG_MISSING
                                                )
              )
            ).
          return.
        endif.
      endif.
    endif.
  endmethod.

  method geoCodeManual.
    read entities of /PLCE/R_PDFunctionalLocation in local mode
      entity PDFunctionalLocation all fields with corresponding #( KEYS )
    result data(LT_RESULTS).

    get time stamp field data(lLastGeocode).
    modify entities of /PLCE/R_PDFunctionalLocation in local mode
      entity PDFunctionalLocation
     update fields ( Latitude Longitude LastGeocode IsManualGeocode GeocodePrecision ) with value #(  for K in KEYS ( %PKY = K-%PKY Longitude = K-%PARAM-Longitude Latitude = K-%PARAM-Latitude LastGeocode = lLastGeocode IsManualGeocode = ABAP_TRUE
        GeocodePrecision = 99 ) )
       failed FAILED
       reported REPORTED.
    RESULT = corresponding #( LT_RESULTS ).

  endmethod.

*  method createFunctionalLocation.
*  endmethod.

  method geoCodeAll.
    select FunctionalLocation from /PLCE/R_PDFunctionalLocation where IsGeocoded is initial into table @data(lt_funcLocs).

    modify entities of /PLCE/R_PDFunctionalLocation ##NO_LOCAL_MODE
      entity PDFunctionalLocation
      execute geoCode from corresponding #( lt_funcLocs )
      result data(LT_RESULTS)
      failed FAILED
      reported REPORTED.

    RESULT = corresponding #( LT_RESULTS ).
  endmethod.

endclass.
