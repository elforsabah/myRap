  METHOD identifycard.
    READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
     ENTITY WeighingSession
     ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_sessions).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).
      " TODO: look up driver/contract by vbeln (from keys[ 1 ]-%param-vbeln)
      SELECT SINGLE * FROM vbak WHERE vbeln = @<ls_session>-Vbeln INTO @DATA(ls_weighsessions).

      IF sy-subrc = 0.
*        <ls_session>-step1ok = abap_true.
        APPEND VALUE #(
         %msg = new_message(
           id       = 'ZWR_WEIGHBRIGE_MESS'
           number   = '000'
           severity = if_abap_behv_message=>severity-success

           v1       = 'Contract is valid'

         )
       ) TO reported-weighingsession.

      ELSE.

*        <ls_session>-step1ok = abap_true.
          APPEND VALUE #(
           %msg = new_message(
             id       = 'ZWR_WEIGHBRIGE_MESS'
             number   = '000'
             severity = if_abap_behv_message=>severity-success

             v1       = 'Contract is not valid'

           )
         ) TO reported-weighingsession.

        ENDIF.
*
        MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
          ENTITY WeighingSession UPDATE FIELDS ( step1ok ) WITH VALUE #( ( %tky = <ls_session>-%tky step1ok = abap_true ) ).
        result = VALUE #( ( %tky = <ls_session>-%tky ) ).
      ENDLOOP.
    ENDMETHOD.
