   READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
      ENTITY Session
      ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(sessions) FOR UPDATE.

    LOOP AT sessions ASSIGNING FIELD-SYMBOL(<s>).
      " TODO: look up driver/contract by CardId (from keys[ 1 ]-%param-CardId)
      <s>-step1_ok = abap_true.
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY Session UPDATE FIELDS ( step1_ok ) WITH VALUE #( ( %tky = <s>-%tky step1_ok = abap_true ) ).
      result = VALUE #( ( %tky = <s>-%tky ) ).
    ENDLOOP.
