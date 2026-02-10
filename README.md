extension implementation in class zbp_e_bp_r_pdmnlservice_ext unique;

extend  behavior for Service
{

action ( precheck, features : global ) assignworkarea parameter ZAE_D_WORKAREA_AB result [0..*] $self;
action ( precheck, features : global ) adjusttime parameter ZAE_D_adjusttime_AB result [0..*] $self;

 determination Setfachbereich on modify { create; update; }

}

extend behavior for ServiceTask
{
}

extend behavior for ExtCustom
{
mapping for /plce/tpdsrvcst  {
    zz_tech_fachbe = zz_tech_fachbe;
    zz_vehicleinfo = zz_vehicleinfo;
    zz_discrepancy = zz_discrepancy;
    zz_reactiontime =  zz_reactiontime;
    zz_order_date = zz_order_date;
    zz_timeadjustment = zz_timeadjustment;
 }

field ( readonly : update ) zz_tech_fachbe;
field ( readonly : update ) zz_vehicleinfo;
field ( readonly : update ) zz_discrepancy;
field ( readonly : update ) zz_reactiontime;
field ( readonly : update ) zz_order_date;
field ( readonly : update ) zz_timeadjustment;
}


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

     methods Setfachbereich for determine on modify
      importing KEYS for Service~Setfachbereich.
     methods adjusttime for modify
       importing keys for action Service~adjusttime result result.
     methods precheck_adjusttime for precheck
       importing keys for action Service~adjusttime.

ENDCLASS.

CLASS lhc_service IMPLEMENTATION.

METHOD Setfachbereich.

 READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      FIELDS ( ReferenceId ServiceUUID ) " // Use CDS casing; add ServiceUUID for key
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services)
      FAILED DATA(lt_failed).

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA lv_zz_tech_fachbe TYPE ewa_order_object .
      SELECT SINGLE zz_tech_fachbe
        FROM ewa_order_object
        WHERE POBJNR = @<ls_service>-ReferenceInternalId
        INTO @lv_zz_tech_fachbe.

      IF sy-subrc = 0 AND lv_zz_tech_fachbe IS NOT INITIAL.
*     " // Check if ExtCustom exists; read it first
        READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY ExtCustom
          FIELDS ( ServiceUUID )  "// Minimal to check existence
          WITH VALUE #( ( ServiceUUID = <ls_service>-ServiceUUID ) )
          RESULT DATA(lt_extcustom).

        IF lt_extcustom IS INITIAL.
*        // Create if missing
          DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
                lt_ext_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_ExtCustom.
          APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
          <ls_ext_create>-ServiceUUID = <ls_service>-ServiceUUID. " // %tky or key
          <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

          MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
            ENTITY Service
            CREATE BY \_ExtCustom
            AUTO FILL CID SET FIELDS WITH lt_ext_create
            MAPPED DATA(lmapped)
            FAILED DATA(lfailed)
            REPORTED DATA(lreported).
*        // Handle errors if needed
        ENDIF.

*      // Now update the field
        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY ExtCustom
          UPDATE FIELDS ( zz_tech_fachbe )
          WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
                          zz_tech_fachbe = lv_zz_tech_fachbe ) )
          FAILED DATA(lt_failed_update)
          REPORTED DATA(lt_reported_update).

          " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
          ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      CLEAR: lt_ext_create,  lt_reported_update,  lt_failed_update,  lreported, lfailed, lmapped.
      ENDIF.
     ENDLOOP.
 ENDMETHOD.

  METHOD get_global_features.
  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD assignworkarea.

    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
    ENTITY Service
    FIELDS ( ReferenceId ServiceUUID ) " // Use CDS casing; add ServiceUUID for key
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services)
    FAILED DATA(lt_failed).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_service>).
*     " // Check if ExtCustom exists; read it first
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        FIELDS ( ServiceUUID )  "// Minimal to check existence
        WITH VALUE #( ( ServiceUUID = <ls_service>-ServiceUUID ) )
        RESULT DATA(lt_extcustom).

      IF lt_extcustom IS INITIAL.
*        // Create if missing
        DATA: lv_cid        TYPE string VALUE '$abap_cid1_',
              lt_ext_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_ExtCustom.
        APPEND INITIAL LINE TO lt_ext_create ASSIGNING FIELD-SYMBOL(<ls_ext_create>).
        <ls_ext_create>-ServiceUUID = <ls_service>-ServiceUUID. " // %tky or key
        <ls_ext_create>-%target = VALUE #( ( %cid = lv_cid ) ).

        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service
          CREATE BY \_ExtCustom
          AUTO FILL CID SET FIELDS WITH lt_ext_create
          MAPPED DATA(lmapped)
          FAILED DATA(lfailed)
          REPORTED DATA(lreported).
*        // Handle errors if needed
      ENDIF.

*      // Now update the field
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY ExtCustom
        UPDATE FIELDS ( zz_tech_fachbe )
        WITH VALUE #( ( %key-ServiceUUID = <ls_service>-ServiceUUID
                        zz_tech_fachbe = <ls_service>-%param-WorkArea ) )
        FAILED DATA(lt_failed_update)
        REPORTED DATA(lt_reported_update).


      " Step 4: Set action result ($self = Service instances). This ensures that the values are shown on the UI
      READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service
          ALL FIELDS WITH CORRESPONDING #( keys )
        RESULT DATA(lt_service_result).

      result = VALUE #( FOR ls_service IN lt_service_result ( %tky   = ls_service-%tky
                                                             %param  = ls_service ) ).
      CLEAR:   lt_ext_create,  lt_reported_update,  lt_failed_update,  lreported, lfailed, lmapped.
    ENDLOOP.
  ENDMETHOD.

  METHOD precheck_assignworkarea.

    LOOP AT keys INTO DATA(ls_service).
*      " Check if WorkArea is valid (query value help entity if needed).
*      IF ls_service-%param-WorkArea IS INITIAL.
*        APPEND VALUE #( %key = ls_service-%key
*                        %msg = new_message( id = 'Z_MSG_CLASS' number = '000' severity = if_abap_behv_message=>severity-error ) )
*               TO reported-Service.
*      ENDIF.

    ENDLOOP.
  ENDMETHOD.
  METHOD adjusttime.

  ENDMETHOD.

  METHOD precheck_adjusttime.
  ENDMETHOD.

ENDCLASS.
