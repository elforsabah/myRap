CLASS lsc_pdservice_ext DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.
    METHODS save_modified REDEFINITION.
ENDCLASS.

CLASS lsc_pdservice_ext IMPLEMENTATION.
  METHOD save_modified.
    " This method is called by the RAP framework during the COMMIT phase.
    " It is now legally safe to write to external database tables!
    
    zcl_wr_waste_order_api=>execute_deferred_cancels( ).
    
  ENDMETHOD.
ENDCLASS.


METHOD terminateService.
    DATA: assigned_services TYPE TABLE FOR ACTION IMPORT /PLCE/R_PDTour\\ServiceAssignment~UnAssign,
          lt_service_update TYPE TABLE FOR UPDATE /PLCE/R_PDService,
          lt_history_create TYPE TABLE FOR CREATE /PLCE/R_PDService\_StatusHistory.

    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service FIELDS ( ServiceUUID ReferenceInternalId ServiceStatus ServiceId )
      WITH CORRESPONDING #( keys ) RESULT DATA(lt_services).

    " ... [Keep your existing Tour Unassign logic here] ...

    LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).
      DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

      " 1. Register the cancellation in memory (NO DB UPDATE YET)
      zcl_wr_waste_order_api=>register_cancel_request(
        iv_pobjnr      = <ls_service>-ReferenceInternalId
        iv_reason_code = ls_param-definiertegrund
        iv_reason_text = ls_param-stornogrund
      ).

      " 2. Prepare bulk RAP updates
      APPEND VALUE #( %tky = <ls_service>-%tky
                      ServiceStatus = 'CANC' ) TO lt_service_update.

      APPEND VALUE #( %tky = <ls_service>-%tky
                      %target = VALUE #( ( %cid = |HIST_{ sy-tabix }| 
                                           ServiceStatus = 'CANC' ) ) ) TO lt_history_create.
      
      APPEND NEW /plce/cx_pd_exception( 
                   severity = if_abap_behv_message=>severity-success
                   textid   = /plce/cx_pd_exception=>service_assigned 
                   serviceid = <ls_service>-ServiceId ) TO reported-%other.
    ENDLOOP.

    " 3. Execute RAP Updates
    IF lt_service_update IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
        ENTITY Service UPDATE FIELDS ( ServiceStatus ) WITH lt_service_update
        CREATE BY \_StatusHistory SET FIELDS WITH lt_history_create.
    ENDIF.

    " 4. Return results
    READ ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
      ENTITY Service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

    result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).
  ENDMETHOD.
