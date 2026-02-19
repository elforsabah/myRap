  method /PLCE/IF_BGPROCESSING_UNIT~EXECUTE.

    data:
      LPROCESSOR type ref to /PLCP/CL_WA_CONFIRMATION,
      LEX        type ref to /PLCE/CX_BASEEXCEPTION.

    try.
        create object LPROCESSOR.
        LPROCESSOR->CONFIRM_TOURS(
          exporting
            IV_PROFILE = PROFILE
            IT_KEYS    = KEYS
        ).
      catch /PLCE/CX_BASEEXCEPTION into LEX.
        raise exception type /PLCE/CX_BGP_UNIT
          exporting
            PREVIOUS = LEX.
    endtry.

  endmethod.



  method READ_TOUR_DATA_CONFIRM.

    data:
      LDATE       type DATS,
      LTIME       type TIMS,
      LCONFNOTES  type standard table of /PLCE/R_PDConfNote,
      LTOURRESREF type ref to /PLCE/SWR_TOUR_RSLT.

    if LINES( IT_KEYS ) is not initial.
      select TourUUID, ResourceId  from /PLCE/R_PDMTour
        for all entries in @IT_KEYS
        where TourUUID = @IT_KEYS-TOUR_UUID
        into table @data(LT_KEYS).

      if LINES( LT_KEYS ) is not initial.

        read entity /PLCE/R_PDMTour
          all fields with corresponding #( LT_KEYS )
          result data(LPDMTOURS)
          by \_ConfirmationNotes all fields with corresponding #( LT_KEYS )
          result data(LCONFNOTESRSLT)
*      by \_ServiceResults all fields with corresponding #( LT_KEYS )
*      result data(LSR)
          by \_Activities all fields with corresponding #( LT_KEYS )
          result data(LACTIVITYRESULTS)
*      by \_AdditionalResults all fields with corresponding #( IT_KEYS mapping TourUUID = TOUR_UUID )
*      result data(LADDRESULTS)
          by \_Tour all fields with corresponding #( LT_KEYS )
          result data(LTOURS)
          by \_Weighings all fields with corresponding #( LT_KEYS )
          result data(LWEIGHRESULTS)
          by \_DocumentResults all fields with corresponding #( LT_KEYS )
          result data(LPDMDOCRES).


        read entity /PLCE/R_MDResource
          all fields with corresponding #( LACTIVITYRESULTS )
          result data(LMDRESSOURCES).

        read entity /PLCE/R_PDTour
          by \_Attachments all fields with corresponding #( LPDMTOURS )
          result data(LTOURATTACHMENTS).

        if lines( LPDMDOCRES ) is not initial.
          delete LPDMDOCRES where ServiceUUID is not initial.
          LTOURATTACHMENTS = corresponding #( base ( LTOURATTACHMENTS ) LPDMDOCRES
            mapping
              Attachment = Attachment
              AttachmentUUID = DocumentResultUUID
              Comments = Comments
              Filename = FileName
              MimeType = MimeType
              ).
        endif.


        select * from /PLCE/R_PDConfNote into table @LCONFNOTES.

        loop at LTOURS reference into data(LTOURREF).
          insert initial line into table ET_DATA reference into LTOURRESREF.
          LTOURRESREF->TOURID = LTOURREF->TOURID.
          LTOURRESREF->ADDITIONALTEXT = LTOURREF->ADDITIONALTEXT.
          LTOURRESREF->TOURTEMPLATE = LTOURREF->TourTemplate.
          select single ExternalTourTemplateID from /PLCE/R_PDTourTemplate into @LTOURRESREF->ExternalTourTemplateID
            where TourTemplate = @LTOURREF->TourTemplate.

**** get order date
          loop at LACTIVITYRESULTS reference into data(LACTREF) using key ID where TourUUID = LTOURREF->TOURUUID and SERVICEUUID is initial and TASKUUID is initial.
            insert initial line into table LTOURRESREF->ACTIVITIES reference into data(LACTIVITYREF).

            convert time stamp LACTREF->ACTIVITYEND time zone SY-ZONLO
              into date LDATE time LTIME.

            LACTIVITYREF->* = corresponding #( LACTREF->*  ).
            LACTIVITYREF->ACTIVITYDATE = LDATE.

*** todo mehrere Tage auf einer Tour!!!
            LTOURRESREF->TOURDATE = LACTREF->ACTIVITYEND.

            read table LMDRESSOURCES reference into data(LMDRESSOURCEREF) with table key ENTITY components ResourceId = LACTREF->ResourceId.
            if SY-SUBRC is initial.
              LACTIVITYREF->RESSOURCETYPE = LMDRESSOURCEREF->SemanticContext.
              LACTIVITYREF->EQUIPMENT = LMDRESSOURCEREF->Equipment.
              LACTIVITYREF->PARTNER = LMDRESSOURCEREF->Partner.
*        LACTIVITYREF->PERSON =

              if LACTIVITYREF->PARTNER is not initial.
                select single cast( BPIdentificationNumber as numc( 8 ) ) from I_BPUsrExternalID
                  into @LACTIVITYREF->PERSON
                  where BusinessPartner = @LACTIVITYREF->PARTNER
                  and BPIdentificationType = 'HCM001'.
              endif.
            endif.
          endloop.

          if LTOURRESREF->TOURDATE is initial.
*      LTOURRESREF->TOURDATE = LTOURREF->StartDate.
            convert date LTOURREF->StartDate into time stamp LTOURRESREF->TOURDATE time zone SY-ZONLO.
          endif.

**** Kopfwiegenoten
          loop at LWEIGHRESULTS reference into data(LWEIGHREF) using key ID where TourUUID = LTOURREF->TOURUUID and ServiceUUID is initial and TaskUUID is initial.
            insert initial line into table LTOURRESREF->WEIGHINGS reference into data(LTOURWEIGHREF).
            LTOURWEIGHREF->* =  corresponding #( LWEIGHREF->* ).
            select single VEH~VEHICLELICENSEPLATE from /PLCE/R_MDVehicle as VEH
              inner join /PLCE/R_MDResource as RES
                on RES~Equipment = VEH~Vehicle
              where RES~ResourceId = @LWEIGHREF->ResourceId
              into @LTOURWEIGHREF->VEHICLELICENSEPLATE.

          endloop.

*** Confirmation notes
*** Additional Result
*** Attachment von Tour und Confirmation Note

          loop at LCONFNOTESRSLT reference into data(LCONFNOTEREF) using key ID where TourUUID = LTOURREF->TOURUUID and ServiceUUID is initial. " and TaskUUID is initial.
            insert initial line into table LTOURRESREF->CONFNOTES reference into data(LTOURCONFNOTEREF).
            LTOURCONFNOTEREF->* = corresponding #( LCONFNOTEREF->* mapping ATTACHMENTUUID = ConfirmationNoteUUID ).
            read table LCONFNOTES reference into data(LC) with key ConfirmationNote = LCONFNOTEREF->ConfirmationNote.
            if SY-SUBRC is initial.
              LTOURCONFNOTEREF->EXTERNALID = LC->ExternalId.
            endif.
          endloop.

          loop at LTOURATTACHMENTS using key Entity reference into data(LATTACHMENTREF) where TourUUID = LTOURREF->TOURUUID.
            insert initial line into table LTOURRESREF->ATTACHMENTS reference into data(LTOURATTACHMENTREF).
            LTOURATTACHMENTREF->* = corresponding #( LATTACHMENTREF->* ).
          endloop.

        endloop.

      endif.
    endif.

  endmethod.



  method WRITE_DATA_TO_EEWA.

    data:
      LCALLBACK type ref to CL_EEWA_IMPL_BO_CALLBACK_MLOG,
      LBAPIRET  type BAPIRET2.

    create object LCALLBACK exporting PAR_INIT = SPACE.
    if LINES( IT_TOUR_DATA ) is not initial.
      LCALLBACK->MSGLOG->HEAD-EXTNUMBER = |{ IT_TOUR_DATA[ 1 ]-TOURID } { IT_TOUR_DATA[ 1 ]-TOURDATE }|.
    endif.
    LCALLBACK->INIT( ).
    LCALLBACK->MSGLOG->SETCUMULATE( PAR_CUMULATE = 'X' ).

    try.

        TA_BEGIN_NAMED_VAR TA /PLCP/CL_TA_WA_ORDER_RSLT=>CTA_ORDER_RESULT.
        TA_DO_COMMIT TA.
*        TA_NO_COMMIT TA.
        TA_DO_RAISE_EXCEPTION TA.
        TA_SET_CALLBACK TA LCALLBACK.
        TA_SET_PARAM TA RESULTDATAS IT_TOUR_DATA.
        TA_SET_PARAM TA USER_ID IS_SEQUENCE_CONTROL-USER_ID.
        if IS_SEQUENCE_CONTROL is initial.
          TA_EXECUTE TA BOOK_IMPORT_RESULTS.
        else.
          if IS_SEQUENCE_CONTROL-SEQUENCE_OPERATIONAL is not initial.
             TA_EXECUTE TA BOOK_IMPORT_ITEM_SEQUENCES.
          endif.
          if IS_SEQUENCE_CONTROL-SEQUENCE_MASTERDATA is not initial.
            TA_EXECUTE TA BOOK_IMPORT_SERVICE_SEQUENCES.
          endif.
        endif.
        TA_END TA.

      catch CX_EEWA_BASE into data(LEX).
        data(LTEXT) = LEX->GET_TEXT( ).
        LEX->GET_AS_BAPIRET(
          changing
            PAR_BAPIRET2    = LBAPIRET
        ).
        insert LBAPIRET into table CT_BAPIRET.
    endtry.

    if LCALLBACK->MSGLOG->EXISTS_LOG( ) is not initial.
      LCALLBACK->MSGLOG->READ_INTO_BAPIRET(
        importing
          PAR_BAPIRET2       = CT_BAPIRET
      ).
      LCALLBACK->MSGLOG->SAVE( ).
    endif.

  endmethod.

  
