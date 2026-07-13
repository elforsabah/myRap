  METHOD sicherheitb_aendern.

    " 1) neuen Sicherheitsbestand je selektierter Instanz setzen
    MODIFY ENTITIES OF zi_containerstock IN LOCAL MODE
      ENTITY ContainerStock
        UPDATE FIELDS ( SafetyStock )
        WITH VALUE #( FOR key IN keys
                      ( %tky        = key-%tky
                        SafetyStock = key-%param-SafetyStock ) )
      FAILED   failed
      REPORTED reported.

    " 2) geänderte Instanzen zurücklesen und als Ergebnis ($self) liefern
    READ ENTITIES OF zi_containerstock IN LOCAL MODE
      ENTITY ContainerStock
        ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_read).

    result = VALUE #( FOR row IN lt_read
                      ( %tky   = row-%tky
                        %param = row ) ).

  ENDMETHOD.
