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
  METHOD get_global_features. ENDMETHOD.
  METHOD get_global_authorizations. ENDMETHOD.

  METHOD precheck_assignworkarea.
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
        ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service).
    LOOP AT lt_service INTO DATA(ls_service).
      IF ls_service-%param-WorkArea IS INITIAL.
        APPEND VALUE #( %key = ls_service-%key
                        %msg = new_message( id = 'Z_MSG_CLASS' number = '001' severity = if_abap_behv_message=>severity-error
                                            v1 = 'WorkArea is mandatory' ) )
               TO reported-Service.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.

  METHOD assignworkarea.
    DATA: lt_extcustom_update TYPE TABLE FOR UPDATE /PLCE/R_PDService \\ ExtCustom.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      APPEND VALUE #( %key-ServiceUUID = <ls_key>-ServiceUUID
                      zz_tech_fachbe = <ls_key>-%param-WorkArea )
             TO lt_extcustom_update.
    ENDLOOP.
    MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
        UPDATE BY \_ExtCustom
        FIELDS ( zz_tech_fachbe )
        WITH lt_extcustom_update
      FAILED DATA(lt_failed)
      REPORTED DATA(lt_reported).
    reported = CORRESPONDING #( DEEP lt_reported ).
    failed   = CORRESPONDING #( DEEP lt_failed ).
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
        ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_read)
      FAILED failed
      REPORTED reported.
    result = VALUE #( FOR ls_read IN lt_read
                      ( %key = ls_read-%key
                        %data = ls_read-%data ) ).
  ENDMETHOD.
ENDCLASS.
