CLASS lhc_service DEFINITION INHERITING FROM cl_abap_behavior_handler.

  PRIVATE SECTION.

    METHODS get_global_features FOR GLOBAL FEATURES
      IMPORTING REQUEST requested_features FOR Service RESULT result.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Service RESULT result.

    METHODS assignworkarea FOR MODIFY
      IMPORTING keys FOR ACTION Service~assignworkarea RESULT result.

    METHODS precheck_assignworkarea FOR PRECHECK
      IMPORTING keys FOR ACTION Service~assignworkarea.

ENDCLASS.

CLASS lhc_service IMPLEMENTATION.

  METHOD get_global_features.
  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD assignworkarea.

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_service>).
        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY ExtCustom
          UPDATE FIELDS ( zz_tech_fachbe )
          WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
                          zz_tech_fachbe = <ls_service>-%param-WorkArea ) )
          FAILED DATA(lt_failed_update)
          REPORTED DATA(lt_reported_update).
       ENDLOOP.
  ENDMETHOD.

  METHOD precheck_assignworkarea.
  ENDMETHOD.
ENDCLASS.
