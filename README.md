 READ ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
      ENTITY Session BY \_itself FIELDS ( load_type ) WITH CORRESPONDING #( keys )
      RESULT DATA(sessions) FOR UPDATE.

    LOOP AT sessions ASSIGNING FIELD-SYMBOL(<s>).
      <s>-load_type = keys[ sy-tabix ]-%param-LoadType.
      <s>-step2_ok  = abap_true.
      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
        ENTITY Session UPDATE FIELDS ( load_type step2_ok )
        WITH VALUE #( ( %tky = <s>-%tky load_type = <s>-load_type step2_ok = abap_true ) ).
      result = VALUE #( ( %tky = <s>-%tky ) ).
    ENDLOOP.
