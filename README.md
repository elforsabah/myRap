CLASS lhc_containerstock DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    " von "authorization master ( global )" gefordert:
    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR ContainerStock
      RESULT result.

    METHODS validateSafetyStock FOR VALIDATE ON SAVE
      IMPORTING keys FOR ContainerStock~validateSafetyStock.
ENDCLASS.

CLASS lhc_containerstock IMPLEMENTATION.

  METHOD get_global_authorizations.
    " Steuert das Ändern des Sicherheitsbestands. Prototyp: erlaubt.
    IF requested_authorizations-%update = if_abap_behv=>mk-on.
*     AUTHORITY-CHECK OBJECT 'Z_CONT_ST'
*       ID 'ACTVT' FIELD '02'.
*     IF sy-subrc = 0.
        result-%update = if_abap_behv=>auth-allowed.
*     ELSE.
*       result-%update = if_abap_behv=>auth-unauthorized.
*     ENDIF.
    ENDIF.
  ENDMETHOD.

  METHOD validateSafetyStock.

    READ ENTITIES OF zi_containerstock IN LOCAL MODE
      ENTITY ContainerStock
        FIELDS ( SafetyStock )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_stock).

    LOOP AT lt_stock INTO DATA(ls_stock).
      IF ls_stock-SafetyStock < 0.
        APPEND VALUE #( %tky = ls_stock-%tky ) TO failed-containerstock.
        APPEND VALUE #( %tky                 = ls_stock-%tky
                        %element-SafetyStock = if_abap_behv=>mk-on
                        %msg = new_message_with_text(
                                 severity = if_abap_behv_message=>severity-error
                                 text     = 'Sicherheitsbestand darf nicht negativ sein' )
                      ) TO reported-containerstock.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

ENDCLASS.
