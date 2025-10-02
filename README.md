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
    " TODO: Implement if needed (e.g., result-%action-assignworkarea = if_abap_behv=>fc-o-enabled ).
  ENDMETHOD.

  METHOD get_global_authorizations.
    " TODO: Implement authorization checks (e.g., result-%action-assignworkarea = if_abap_behv=>auth-allowed ).
  ENDMETHOD.

  METHOD precheck_assignworkarea.
    " Sample: Validate WorkArea parameter (e.g., exists in value help entity).
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
        ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service).

    LOOP AT lt_service INTO DATA(ls_service).
      " Check if WorkArea is valid (query value help entity if needed).
      IF ls_service-%param-WorkArea IS INITIAL.
        APPEND VALUE #( %key = ls_service-%key
                        %msg = new_message( id = 'Z_MSG_CLASS' number = '001' severity = if_abap_behv_message=>severity-error
                                            v1 = 'WorkArea is mandatory' ) )
               TO reported-Service.
      ENDIF.
      " Add more checks: e.g., user auth, business rules.
    ENDLOOP.
  ENDMETHOD.

  METHOD assignworkarea.
    " Step 1: Collect updates for ExtCustom (child of Service).
    DATA: lt_extcustom_update TYPE TABLE FOR UPDATE /PLCE/R_PDService \\ ExtCustom.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Assume ExtCustom exists; if not, add CREATE logic below.
      APPEND VALUE #( %key-ServiceUUID = <ls_key>-ServiceUUID  " Key from parent Service
                      zz_tech_fachbe = <ls_key>-%param-WorkArea )  " e.g., 'TEC'
             TO lt_extcustom_update.
    ENDLOOP.

    " Step 2: Single MODIFY for efficiency (path: Service | ExtCustom).
    MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
        UPDATE BY \_ExtCustom  " Path to child
        FIELDS ( zz_tech_fachbe )  " Only update this field
        WITH lt_extcustom_update
      FAILED DATA(lt_failed)
      REPORTED DATA(lt_reported).

    " Step 3: Map errors to action's reported/failed (propagate to UI).
    reported = CORRESPONDING #( DEEP lt_reported ).
    failed   = CORRESPONDING #( DEEP lt_failed ).

    " Step 4: If no failures, read updated Service (incl. _ExtCustom) for result.
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
        ALL FIELDS WITH CORRESPONDING #( keys )  " Re-read affected keys
      RESULT DATA(lt_read)
      FAILED failed
      REPORTED reported.

    " Step 5: Map to result ($self = Service with updated child).
    result = VALUE #( FOR ls_read IN lt_read
                      WHERE ( NOT %key IS INITIAL )  " Filter valid reads
                      ( %key = ls_read-%key
                        %data = ls_read-%data ) ).  " %data includes _ExtCustom.zz_tech_fachbe

    " Optional: Handle creation if ExtCustom doesn't exist (e.g., after failed UPDATE).
    " LOOP AT lt_failed-Service INTO DATA(ls_failed).
    "   IF line_exists( ls_failed-%update->ExtCustom[ ServiceUUID = ls_failed-%key-ServiceUUID ] ).
    "     " Creation logic here: APPEND to lt_extcustom_create, then MODIFY CREATE.
    "   ENDIF.
    " ENDLOOP.
  ENDMETHOD.

ENDCLASS.
