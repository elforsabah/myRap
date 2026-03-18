METHOD terminateService.
    DATA: assigned_services TYPE TABLE FOR ACTION IMPORT /PLCE/R_PDTour\\ServiceAssignment~UnAssign,
          lt_bapiret        TYPE bapirettab.

    " 1. Read the current services to get the Reference ID (Waste Order)
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      FIELDS ( ServiceUUID ReferenceId ServiceStatus )
      WITH CORRESPONDING #( keys )
      RESULT DATA(lt_services).

    " 2. Find if these services are assigned to a tour
    SELECT asgmt~ServiceUUID, asgmt~TourUUID 
      FROM /PLCE/R_PDTourServiceAsgmt AS asgmt
      INNER JOIN @keys AS keys ON asgmt~ServiceUUID = keys~ServiceUUID 
      INTO CORRESPONDING FIELDS OF TABLE @assigned_services.

    " 3. Unassign from Tour (if planned)
    IF assigned_services IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDTour
        ENTITY ServiceAssignment
          EXECUTE UnAssign FROM assigned_services
        FAILED DATA(srvc_unassign_failed)
        REPORTED DATA(srvc_unassign_reported).

      " Pass messages to reported
      /plce/cl_base_misc=>check_response(
        EXPORTING is_response = srvc_unassign_reported
        CHANGING  ct_messages = reported-%other
      ).
    ENDIF.

    " 4. Process each selected service
    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      " Get parameters from the popup (ZAE_D_TERMINATE_SERVICE)
      DATA(ls_param) = keys[ KEY entity %tky = <ls_service>-%tky ]-%param.

      " Call Reusable Logic to cancel Waste Order Item
      DATA(lv_success) = /plce/cl_waste_order_api=>cancel_waste_order_item(
                           iv_order_reference = <ls_service>-ReferenceId
                           iv_reason_code     = ls_param-definiertegrund
                           iv_reason_text     = ls_param-stornogrund
                           IMPORTING et_messages = lt_bapiret ).

      IF lv_success = abap_true.
        " 5. Update Service Status to remove it from active planning
        " Assuming you have a constant for 'CANCELLED' status
        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service
            UPDATE FIELDS ( ServiceStatus ) 
            WITH VALUE #( ( %tky = <ls_service>-%tky 
                            ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-cancelled ) ) " <-- Adjust constant
            CREATE BY \_StatusHistory AUTO FILL CID
            SET FIELDS WITH VALUE #( ( %tky = <ls_service>-%tky
                                       %target = VALUE #( ( ServiceStatus = /plce/if_pd_constants=>c_pd_service_status-cancelled ) ) ) ).
        
        " Append Success Message
        APPEND NEW /plce/cx_pd_exception( 
                     severity = if_abap_behv_message=>severity-success
                     textid   = /plce/cx_pd_exception=>cs_service_cancelled " <-- Add to your message class
                     serviceid = <ls_service>-ServiceId ) TO reported-%other.
      ELSE.
        " Map BAPI errors to RAP reported/failed structures
        /plce/cl_base_misc=>check_bapiret(
          EXPORTING it_bapiret  = lt_bapiret
          CHANGING  cs_reported = reported
        ).
        APPEND VALUE #( %tky = <ls_service>-%tky ) TO failed-service.
      ENDIF.
    ENDLOOP.

    " 6. Return the updated services
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service
      ALL FIELDS WITH CORRESPONDING #( keys )
      RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

  ENDMETHOD.
