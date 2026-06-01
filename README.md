METHOD anlageaendern.

  DATA: lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
        lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom.

  " ✅ NEW: Collect service UUIDs for tour lookup
  DATA: lt_service_uuids TYPE TABLE OF /plce/de_service_uuid.

  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service
    FIELDS ( serviceuuid referenceinternalid referenceid )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
    DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

    " ✅ Collect successful service UUIDs for later tour processing
    APPEND <ls_service>-serviceuuid TO lt_service_uuids.

    " ... your existing code for ExtCustom create/update ...

  ENDLOOP.

  " ... your existing bulk create and update logic ...

  " =========================================================================
  " ✅ NEW: After successful update, recalculate tour routes
  " =========================================================================
  IF lt_failed_update IS INITIAL AND lt_service_uuids IS NOT INITIAL.
    
    " Find tours assigned to these services
    SELECT DISTINCT tour_uuid
      FROM /plce/tpdsrvtsk
      WHERE service_uuid IN @lt_service_uuids
        AND tour_uuid IS NOT INITIAL
      INTO TABLE @DATA(lt_tour_uuids).

    IF lt_tour_uuids IS NOT INITIAL.
      " Prepare keys for GenerateGeoRoute action
      DATA: lt_tour_keys TYPE TABLE FOR ACTION IMPORT /plce/r_pdtour~generategeoroute.

      LOOP AT lt_tour_uuids INTO DATA(lv_tour_uuid).
        APPEND VALUE #( %tky-touruuid = lv_tour_uuid ) TO lt_tour_keys.
      ENDLOOP.

      " Call GenerateGeoRoute action on all affected tours
      MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
        ENTITY tour
          EXECUTE generategeoroute FROM lt_tour_keys
        FAILED DATA(lt_tour_failed)
        REPORTED DATA(lt_tour_reported).

      " Add information message about tour recalculation
      IF lt_tour_failed IS INITIAL.
        LOOP AT lt_services ASSIGNING <ls_service>.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '017'  " New message: "Spur wurde neu berechnet"
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = <ls_service>-referenceid )
                        ) TO reported-service.
        ENDLOOP.
      ELSE.
        " Log tour recalculation failures (but don't fail the action)
        LOOP AT lt_tour_reported-tour ASSIGNING FIELD-SYMBOL(<ls_tour_msg>).
          APPEND VALUE #( %tky = lt_services[ 1 ]-%tky
                          %msg = <ls_tour_msg>-%msg
                        ) TO reported-service.
        ENDLOOP.
      ENDIF.
    ENDIF.
  ENDIF.

  " Return updated data to UI
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

  result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

ENDMETHOD.
