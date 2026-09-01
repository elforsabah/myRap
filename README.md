METHOD stornobmsservice.

*----------------------------------------------------------------------*
* There is no storno endpoint in the BMS API. Cancellation is the SAME
* create-order-halle call with status "cancelled". The payload is
* replayed from ZBMS_API_LOG so BMS sees exactly the order it accepted.
*
* Prerequisite: ZBMS_API_LOG-REQUEST_PAYLOAD must be type STRING.
* At CHAR 1333 the stored JSON is truncated and replays as invalid.
*----------------------------------------------------------------------*
  TYPES:
    BEGIN OF ty_svc_storno,
      service_uuid     TYPE /plce/pdservice_uuid,
      service_id       TYPE /plce/pdservice_id,
      reference_id     TYPE char30,
      reference_int_id TYPE char30,
    END OF ty_svc_storno,
    tt_svc_storno TYPE HASHED TABLE OF ty_svc_storno
                  WITH UNIQUE KEY service_uuid.

  DATA:
    lt_services     TYPE tt_svc_storno,
    ls_svc          TYPE ty_svc_storno,
    ls_srvcst       TYPE /plce/tpdsrvcst,
    lv_dest         TYPE rfcdest,
    lv_token        TYPE string,
    lv_error        TYPE string,
    lv_http_status  TYPE i,
    lv_response     TYPE string,
    lv_json         TYPE string,
    lv_storno_ok    TYPE i,
    lv_storno_err   TYPE i,
    lv_pobjnr       TYPE j_objnr,
    lv_sto_subrc    TYPE sy-subrc,
    lv_order_number TYPE aufnr.

  DATA: BEGIN OF ls_err_body,
          BEGIN OF error,
            message   TYPE string,
            timestamp TYPE string,
          END OF error,
        END OF ls_err_body.

*----------------------------------------------------------------------*
* CONFIG — endpoint comes from an SM59 destination
*----------------------------------------------------------------------*
  SELECT SINGLE bms_destination, bms_username, bms_password, active
    FROM ztour_bms_cfg
    WHERE config_id = 'DEFAULT'
    INTO @DATA(ls_cfg).

  IF sy-subrc <> 0 OR ls_cfg-active <> abap_true
     OR ls_cfg-bms_destination IS INITIAL.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ky>).
      APPEND VALUE #(
        %tky = <ky>-%tky
        %msg = new_message(
                 id       = 'Z_MSG_SVR_TOUR_EXT'
                 number   = '018'
                 severity = if_abap_behv_message=>severity-error )
      ) TO reported-tour.
      APPEND VALUE #( %tky = <ky>-%tky ) TO failed-tour.
    ENDLOOP.
    RETURN.
  ENDIF.

  " typed local — the DDIC field may not be type-compatible with RFCDEST
  lv_dest = ls_cfg-bms_destination.

  DATA(lv_bms_user)     = CONV string( ls_cfg-bms_username ).
  DATA(lv_bms_password) = CONV string( ls_cfg-bms_password ).

*----------------------------------------------------------------------*
* Tours
*----------------------------------------------------------------------*
  READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
    ENTITY tour
      FIELDS ( tourid touruuid )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tours)
    FAILED DATA(lt_failed).

  LOOP AT lt_failed-tour ASSIGNING FIELD-SYMBOL(<fail>).
    APPEND VALUE #(
      %tky = <fail>-%tky
      %msg = new_message(
               id       = 'Z_MSG_SVR_TOUR_EXT'
               number   = '024'
               severity = if_abap_behv_message=>severity-error )
    ) TO reported-tour.
    APPEND VALUE #( %tky = <fail>-%tky ) TO failed-tour.
  ENDLOOP.

  IF lt_tours IS INITIAL.
    RETURN.
  ENDIF.

*----------------------------------------------------------------------*
* Service assignments
*----------------------------------------------------------------------*
  READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
    ENTITY tour BY \_serviceassignments
      FIELDS ( touruuid serviceuuid removed )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_asgmts).

  DELETE lt_asgmts WHERE removed IS NOT INITIAL.

  IF lt_asgmts IS INITIAL.
    RETURN.
  ENDIF.

  SELECT service_uuid, service_id, reference_id, reference_int_id
    FROM /plce/tpdsrv
    FOR ALL ENTRIES IN @lt_asgmts
    WHERE service_uuid = @lt_asgmts-serviceuuid
    INTO CORRESPONDING FIELDS OF TABLE @lt_services.

*----------------------------------------------------------------------*
* Main loop
*----------------------------------------------------------------------*
  LOOP AT lt_tours INTO DATA(ls_tour).

    CLEAR: lv_storno_ok, lv_storno_err, lv_token, lv_error.

    DATA(lv_tour_id_out) = condense( |{ ls_tour-tourid ALPHA = OUT }| ).

    " --- Authentication --------------------------------------------------
    zcl_wr_pd_tour_helper=>get_bms_bearer_token(
      EXPORTING
        iv_destination = lv_dest
        iv_username    = lv_bms_user
        iv_password    = lv_bms_password
      IMPORTING
        ev_token       = lv_token
        ev_error       = lv_error ).

    IF lv_error IS NOT INITIAL.
      APPEND VALUE #(
        %tky = ls_tour-%tky
        %msg = new_message_with_text(
                 severity = if_abap_behv_message=>severity-error
                 text     = lv_error )
      ) TO reported-tour.
      APPEND VALUE #( %tky = ls_tour-%tky ) TO failed-tour.
      CONTINUE.
    ENDIF.

    " --- service loop ----------------------------------------------------
    LOOP AT lt_asgmts INTO DATA(ls_asgmt)
      WHERE touruuid = ls_tour-touruuid.

      CLEAR: ls_svc, lv_http_status, lv_response, lv_json,
             lv_pobjnr, lv_order_number, lv_sto_subrc.

      READ TABLE lt_services INTO ls_svc
        WITH TABLE KEY service_uuid = ls_asgmt-serviceuuid.

      IF sy-subrc <> 0.
        lv_storno_err = lv_storno_err + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '025'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_tour_id_out )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      DATA(lv_svc_ref_out) = condense( |{ ls_svc-reference_id ALPHA = OUT }| ).

      " Only cancel what was actually released
      SELECT SINGLE zz_bms_status
        FROM /plce/tpdsrvcst
        WHERE service_uuid = @ls_asgmt-serviceuuid
        INTO @DATA(lv_cur_status).

      IF sy-subrc <> 0 OR lv_cur_status <> 'FREIGEGEBEN'.
        CONTINUE.   " never released, or already cancelled — nothing to do
      ENDIF.

      " Replay the payload BMS accepted
      SELECT SINGLE order_number, request_payload
        FROM zbms_api_log
        WHERE tour_uuid    = @ls_tour-touruuid
          AND service_uuid = @ls_asgmt-serviceuuid
        INTO ( @lv_order_number, @lv_json ).

      IF sy-subrc <> 0 OR lv_json IS INITIAL.
        lv_storno_err = lv_storno_err + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '027'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_svc_ref_out )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      " Switch the status. This check is critical: if the pattern does not
      " match, the payload still says "ok" and posting it would CREATE the
      " order again instead of cancelling it.
      REPLACE FIRST OCCURRENCE OF '"status":"ok"'
        IN lv_json WITH '"status":"cancelled"'.

      IF sy-subrc <> 0.
        lv_storno_err = lv_storno_err + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '031'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_svc_ref_out )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      lv_pobjnr = ls_svc-reference_int_id.

      " --- POST — same endpoint as the release ---------------------------
      zcl_wr_pd_tour_helper=>post_bms_order(
        EXPORTING
          iv_destination  = lv_dest
          iv_bearer_token = lv_token
          iv_json         = lv_json
        IMPORTING
          ev_http_status  = lv_http_status
          ev_response     = lv_response ).

      zcl_wr_pd_tour_helper=>log_bms_call(
        iv_tour_uuid    = ls_tour-touruuid
        iv_service_uuid = ls_asgmt-serviceuuid
        iv_order_number = lv_order_number
        iv_endpoint     = '/api/container/create-order-halle (CANCEL)'
        iv_http_status  = lv_http_status
        iv_request      = lv_json
        iv_response     = lv_response
        iv_pobjnr       = lv_pobjnr ).

      IF lv_http_status = 200 OR lv_http_status = 201.

        lv_storno_ok = lv_storno_ok + 1.

        " Same FM as the release path — writes /PLCE/TPDSRVCST and
        " EWA_ORDER_OBJECT together in one separate LUW.
        CALL FUNCTION 'ZWR_BMS_UPDATE_SERVICE'
          DESTINATION 'NONE'
          EXPORTING
            service_uuid          = ls_asgmt-serviceuuid
            pobjnr                = lv_pobjnr
            zz_bms_status         = 'STORNIERT'
          IMPORTING
            ev_subrc              = lv_sto_subrc
          EXCEPTIONS
            communication_failure = 1
            system_failure        = 2
            OTHERS                = 3.

        IF sy-subrc <> 0 OR lv_sto_subrc <> 0.
          CLEAR ls_srvcst.
          ls_srvcst-service_uuid  = ls_asgmt-serviceuuid.
          ls_srvcst-zz_bms_status = 'STORNIERT'.
          INSERT /plce/tpdsrvcst FROM ls_srvcst.
        ENDIF.

        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '015'
                   severity = if_abap_behv_message=>severity-success
                   v1       = lv_order_number )
        ) TO reported-tour.

      ELSE.

        lv_storno_err = lv_storno_err + 1.

        " 400 carries {"errors":{...}} or {"error":{"message":...}};
        " 500 carries only a generic title plus a traceId. The complete
        " body is always in ZBMS_API_LOG.
        CLEAR ls_err_body.
        TRY.
            /ui2/cl_json=>deserialize(
              EXPORTING json = lv_response
              CHANGING  data = ls_err_body ).
          CATCH cx_root.
            CLEAR ls_err_body.
        ENDTRY.

        DATA(lv_bms_msg) = COND string(
          WHEN ls_err_body-error-message IS NOT INITIAL
          THEN ls_err_body-error-message
          WHEN strlen( lv_response ) > 80
          THEN |HTTP { lv_http_status }: { lv_response(80) }|
          WHEN lv_response IS NOT INITIAL
          THEN |HTTP { lv_http_status }: { lv_response }|
          ELSE |HTTP { lv_http_status }| ).

        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '016'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_order_number
                   v2       = lv_bms_msg )
        ) TO reported-tour.

      ENDIF.

    ENDLOOP.   " services

*----------------------------------------------------------------------*
* Nothing was cancelled, but something was attempted.
*     Both counters zero = no service was in FREIGEGEBEN, a legitimate
*     no-op that must not be reported as a failure.
*----------------------------------------------------------------------*
    IF lv_storno_ok = 0 AND lv_storno_err > 0.
      APPEND VALUE #( %tky = ls_tour-%tky ) TO failed-tour.
      CONTINUE.
    ENDIF.

*----------------------------------------------------------------------*
* Tour status
*     The old code set STORNIERT unconditionally — a tour whose every
*     cancellation failed still showed as cancelled while the orders
*     were still on the drivers' tablets.
*----------------------------------------------------------------------*
    DATA(lv_tour_status) = COND /plce/char20(
      WHEN lv_storno_ok > 0 AND lv_storno_err = 0 THEN 'STORNIERT'
      WHEN lv_storno_ok > 0                       THEN 'PARTIAL'
      WHEN lv_storno_err > 0                      THEN 'ERROR'
      ELSE                                             'STORNIERT' ).

    MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY extcustom
        UPDATE FIELDS ( zz_bms_status )
        WITH VALUE #( ( touruuid      = ls_tour-touruuid
                        zz_bms_status = lv_tour_status ) )
      FAILED   DATA(lf_mod)
      REPORTED DATA(lr_mod).

    IF lf_mod IS NOT INITIAL.
      MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
        ENTITY tour
          CREATE BY \_extcustom
            FIELDS ( zz_bms_status )
            WITH VALUE #( (
              %tky    = ls_tour-%tky
              %target = VALUE #( (
                %cid          = |BMS_STORNO_TGT_{ ls_tour-touruuid }|
                zz_bms_status = lv_tour_status ) ) ) )
        FAILED   DATA(lf_crt)
        REPORTED DATA(lr_crt).

      IF lf_crt IS NOT INITIAL.
        APPEND LINES OF lr_crt-tour TO reported-tour.
      ENDIF.
    ENDIF.

  ENDLOOP.   " tours

ENDMETHOD.
