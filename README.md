  METHOD remove_obsolete_tour.

    CLEAR rv_removed.

    " which forms are we writing again?
    DATA lt_forms TYPE SORTED TABLE OF /plce/text40 WITH UNIQUE KEY table_line.
    LOOP AT it_new ASSIGNING FIELD-SYMBOL(<ls_new>) WHERE comments CP 'ZWRPRT/*'.
      INSERT <ls_new>-comments INTO TABLE lt_forms.
    ENDLOOP.
    CHECK lt_forms IS NOT INITIAL.

    READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY tour BY \_attachments
        FIELDS ( attachmentuuid filename comments )
        WITH VALUE #( ( touruuid = iv_touruuid ) )
      RESULT DATA(lt_existing).

    DATA lt_del TYPE TABLE FOR DELETE /plce/r_pdtour\\attachments.

    LOOP AT lt_existing ASSIGNING FIELD-SYMBOL(<ls_att>).
      CHECK <ls_att>-comments CP 'ZWRPRT/*'.                           " 1. ours?
      CHECK line_exists( lt_forms[ table_line = <ls_att>-comments ] ). " 2. rewriting?
      APPEND VALUE #( touruuid       = iv_touruuid
                      attachmentuuid = <ls_att>-attachmentuuid ) TO lt_del.
    ENDLOOP.

    CHECK lt_del IS NOT INITIAL.

    MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY attachments
        DELETE FROM CORRESPONDING #( lt_del )
      FAILED DATA(lt_fail).

    rv_removed = lines( lt_del ) - lines( lt_fail-attachments ).

  ENDMETHOD.


  METHOD remove_obsolete_service.

    CLEAR rv_removed.

    DATA lt_forms TYPE SORTED TABLE OF /plce/text40 WITH UNIQUE KEY table_line.
    LOOP AT it_new ASSIGNING FIELD-SYMBOL(<ls_new>)
         WHERE serviceuuid = iv_serviceuuid AND comments CP 'ZWRPRT/*'.
      INSERT <ls_new>-comments INTO TABLE lt_forms.
    ENDLOOP.
    CHECK lt_forms IS NOT INITIAL.

    READ ENTITIES OF /plce/r_pdservice
      ENTITY service BY \_attachments
        FIELDS ( attachmentuuid filename comments )
        WITH VALUE #( ( serviceuuid = iv_serviceuuid ) )
      RESULT DATA(lt_existing).

    DATA lt_del TYPE TABLE FOR DELETE /plce/r_pdservice\\attachments.

    LOOP AT lt_existing ASSIGNING FIELD-SYMBOL(<ls_att>).
      CHECK <ls_att>-comments CP 'ZWRPRT/*'.
      CHECK line_exists( lt_forms[ table_line = <ls_att>-comments ] ).
      APPEND VALUE #( serviceuuid    = iv_serviceuuid
                      attachmentuuid = <ls_att>-attachmentuuid ) TO lt_del.
    ENDLOOP.

    CHECK lt_del IS NOT INITIAL.

    MODIFY ENTITIES OF /plce/r_pdservice
      ENTITY attachments
        DELETE FROM CORRESPONDING #( lt_del )
      FAILED DATA(lt_fail).

    rv_removed = lines( lt_del ) - lines( lt_fail-attachments ).

  ENDMETHOD.
