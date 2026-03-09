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
