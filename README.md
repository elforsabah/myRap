CLASS lhc_Tour DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_global_authorizations FOR GLOBAL AUTHORIZATION
      IMPORTING REQUEST requested_authorizations FOR Tour RESULT result.

    METHODS createtour FOR MODIFY
      IMPORTING keys FOR ACTION Tour~createtour RESULT result.

    METHODS precheck_createtour FOR PRECHECK
      IMPORTING keys FOR ACTION Tour~createtour.

    METHODS assign_earliest_to_latest_date FOR DETERMINE ON MODIFY
      IMPORTING keys FOR Tour~assign_earliest_to_latest_date.

    METHODS touranBMSfreigeben FOR MODIFY
      IMPORTING keys FOR ACTION Tour~touranBMSfreigeben RESULT result.

    "-----------------------------------------------------------------------
    " Helper: obtain a Bearer token from the BMS authentication endpoint.
    " Called once per tour (not once per service) to avoid redundant
    " auth calls. Token is valid for the duration of the tour loop.
    "-----------------------------------------------------------------------
    METHODS get_bms_bearer_token
      IMPORTING
        iv_base_url  TYPE string
        iv_username  TYPE string
        iv_password  TYPE string
      EXPORTING
        ev_token     TYPE string
        ev_error     TYPE string.

    "-----------------------------------------------------------------------
    " Helper: POST a single serialized JSON order to the BMS endpoint.
    " Completely independent of the RAP framework — can be called and
    " tested directly without triggering the full action.
    "-----------------------------------------------------------------------
    METHODS post_bms_order
      IMPORTING
        iv_base_url      TYPE string
        iv_bearer_token  TYPE string
        iv_json          TYPE string
      EXPORTING
        ev_http_status   TYPE i
        ev_response      TYPE string.

ENDCLASS.

CLASS lhc_Tour IMPLEMENTATION.

  METHOD assign_earliest_to_latest_date.

    READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour
        FIELDS ( StartDate EndDate )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_tour).

    MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour
        UPDATE FIELDS ( EndDate )
        WITH VALUE #(
          FOR ls IN lt_tour
          WHERE ( StartDate IS NOT INITIAL AND EndDate IS INITIAL )
          ( %tky    = ls-%tky
            EndDate = ls-StartDate )
        )
      FAILED   DATA(lt_fai)
      REPORTED DATA(lt_rep).

  ENDMETHOD.

  METHOD get_global_authorizations.
  ENDMETHOD.

  METHOD createtour.

    DATA lv_first_date   TYPE /plce/date.
    DATA lv_last_date    TYPE /plce/date.
    DATA lv_current_date TYPE /plce/date.
    DATA lv_exists       TYPE abap_bool.

    TYPES: BEGIN OF ty_msg_sort,
             date TYPE /plce/date,
             msg  TYPE REF TO if_abap_behv_message,
           END OF ty_msg_sort.
    DATA lt_msg_sort TYPE STANDARD TABLE OF ty_msg_sort.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>)
         GROUP BY ( template   = <key>-%param-tour_template
                    first_date = <key>-%param-start_date
                    last_date  = <key>-%param-end_date )
         ASSIGNING FIELD-SYMBOL(<group>).

      lv_first_date = <group>-first_date.
      lv_last_date  = COND /plce/date(
                        WHEN <group>-last_date IS INITIAL
                        THEN lv_first_date
                        ELSE <group>-last_date ).

      IF lv_last_date < lv_first_date.
        DATA(lv_tmp)  = lv_first_date.
        lv_first_date = lv_last_date.
        lv_last_date  = lv_tmp.
      ENDIF.

      lv_current_date = lv_first_date.

      WHILE lv_current_date <= lv_last_date.

        CLEAR lv_exists.

        SELECT SINGLE @abap_true
          FROM /plce/r_pdtour
          WHERE TourTemplate = @<group>-template
            AND StartDate    = @lv_current_date
          INTO @lv_exists.

        IF lv_exists = abap_true.

          APPEND VALUE #( date = lv_current_date
                          msg  = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '008'
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = |{ lv_current_date DATE = USER }|
                                   v2       = <group>-template )
                        ) TO lt_msg_sort.

        ELSE.

          DATA(lv_internal_cid) = cl_system_uuid=>create_uuid_x16_static( ).

          MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              EXECUTE createTourWithTemplate
              FROM VALUE #(
                ( %cid                 = lv_internal_cid
                  %param-without_draft = 'X'
                  %param-tour_template = <group>-template
                  %param-start_date    = lv_current_date ) )
            MAPPED   DATA(mapped_tour)
            FAILED   DATA(failed_tour)
            REPORTED DATA(reported_tour).

          READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              ALL FIELDS
              WITH CORRESPONDING #( mapped_tour-tour )
            RESULT DATA(tours).

          IF lines( tours ) > 0.
            APPEND VALUE #( date = lv_current_date
                            msg  = new_message(
                                     id       = 'Z_MSG_SVR_TOUR_EXT'
                                     number   = '007'
                                     severity = if_abap_behv_message=>severity-success
                                     v1       = |{ lv_current_date DATE = USER }|
                                     v2       = |{ tours[ 1 ]-TourId ALPHA = OUT }| )
                          ) TO lt_msg_sort.

            result = VALUE #( BASE result
                              FOR tour IN tours
                              ( %cid   = lv_internal_cid
                                %param = tour ) ).
          ENDIF.
        ENDIF.

        lv_current_date = lv_current_date + 1.

      ENDWHILE.
    ENDLOOP.

    SORT lt_msg_sort BY date DESCENDING.

    LOOP AT lt_msg_sort INTO DATA(ls_msg_sort).
      INSERT ls_msg_sort-msg INTO reported-%other INDEX 1.
    ENDLOOP.

  ENDMETHOD.

  METHOD precheck_createtour.

    DATA(lv_today) = cl_abap_context_info=>get_system_date( ).

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<key>).

      DATA lv_failed   TYPE abap_bool VALUE abap_false.
      DATA lv_start    TYPE /plce/date.
      DATA lv_end      TYPE /plce/date.
      DATA lv_template TYPE /plce/pdtour_template.

      lv_start    = <key>-%param-start_date.
      lv_end      = <key>-%param-end_date.
      lv_template = <key>-%param-tour_template.

      DATA(lv_cid) = cl_system_uuid=>create_uuid_x16_static( ).

      IF lv_start IS INITIAL.
        APPEND VALUE #(
          %cid = <key>-%cid
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '003'
                   severity = if_abap_behv_message=>severity-error
                   v1       = |{ lv_start DATE = USER }| )
        ) TO reported-tour.
        lv_failed = abap_true.
      ENDIF.

      IF lv_end IS NOT INITIAL AND lv_end < lv_start.
        APPEND VALUE #(
          %cid = <key>-%cid
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '002'
                   severity = if_abap_behv_message=>severity-error
                   v1       = |{ lv_start DATE = USER }|
                   v2       = |{ lv_end DATE = USER }| )
        ) TO reported-tour.
        lv_failed = abap_true.
      ENDIF.

      IF lv_template IS INITIAL.
        APPEND VALUE #(
          %cid = <key>-%cid
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '001'
                   severity = if_abap_behv_message=>severity-error )
        ) TO reported-tour.
        lv_failed = abap_true.
      ENDIF.

      IF lv_failed = abap_true.
        APPEND VALUE #( %cid = <key>-%cid ) TO failed-tour.
      ENDIF.

    ENDLOOP.
  ENDMETHOD.

  METHOD get_bms_bearer_token.
    "-----------------------------------------------------------------------
    " Calls POST /api/user/authenticate/user and returns a ready-to-use
    " Authorization header value: "Bearer <token>"
    " Token is ~373 chars; with prefix = 380 chars total.
    "-----------------------------------------------------------------------
    CLEAR: ev_token, ev_error.

    DATA(lv_auth_json) = |\{ "username": "{ iv_username }", | &&
                          |"password": "{ iv_password }" \}|.

    DATA lo_http TYPE REF TO if_http_client.
    cl_http_client=>create_by_url(
      EXPORTING
        url    = iv_base_url && '/api/user/authenticate/user'
      IMPORTING
        client = lo_http
      EXCEPTIONS
        OTHERS = 4 ).

    IF sy-subrc <> 0.
      ev_error = 'BMS: Could not create HTTP client for authentication'.
      RETURN.
    ENDIF.

    lo_http->request->set_method( if_http_request=>co_request_method_post ).
    lo_http->request->set_header_field(
      name  = 'Content-Type'
      value = 'application/json' ).
    lo_http->request->set_cdata( lv_auth_json ).

    lo_http->send(    EXCEPTIONS OTHERS = 4 ).
    lo_http->receive( EXCEPTIONS OTHERS = 4 ).

    DATA(lv_status) = lo_http->response->get_status( ).
    DATA(lv_body)   = lo_http->response->get_cdata( ).
    lo_http->close( ).

    IF lv_status <> 200.
      ev_error = |BMS auth failed — HTTP { lv_status }: { lv_body }|.
      RETURN.
    ENDIF.

    " Deserialize token from response JSON
    TYPES: BEGIN OF ty_auth_response,
             token TYPE string,
           END OF ty_auth_response.
    DATA ls_auth TYPE ty_auth_response.

    /ui2/cl_json=>deserialize(
      EXPORTING json = lv_body
      CHANGING  data = ls_auth ).

    IF ls_auth-token IS INITIAL.
      ev_error = |BMS auth returned no token. Response: { lv_body }|.
      RETURN.
    ENDIF.

    ev_token = |Bearer { ls_auth-token }|.

  ENDMETHOD.

  METHOD post_bms_order.
    "-----------------------------------------------------------------------
    " POSTs a single serialized JSON order to the BMS create-order endpoint.
    " Completely decoupled from the RAP framework — call and test directly.
    "-----------------------------------------------------------------------
    CLEAR: ev_http_status, ev_response.

    DATA lo_http TYPE REF TO if_http_client.
    cl_http_client=>create_by_url(
      EXPORTING
        url    = iv_base_url && '/api/container/create-order-halle'
      IMPORTING
        client = lo_http
      EXCEPTIONS
        OTHERS = 4 ).

    IF sy-subrc <> 0.
      ev_http_status = 0.
      ev_response    = 'BMS: Could not create HTTP client for order POST'.
      RETURN.
    ENDIF.

    lo_http->request->set_method( if_http_request=>co_request_method_post ).
    lo_http->request->set_header_field(
      name  = 'Content-Type'
      value = 'application/json-patch+json' ).
    lo_http->request->set_header_field(
      name  = 'Authorization'
      value = iv_bearer_token ).
    lo_http->request->set_cdata( iv_json ).

    lo_http->send(    EXCEPTIONS OTHERS = 4 ).
    lo_http->receive( EXCEPTIONS OTHERS = 4 ).

    ev_http_status = lo_http->response->get_status( ).
    ev_response    = lo_http->response->get_cdata( ).

    lo_http->close( ).

  ENDMETHOD.

  METHOD touranBMSfreigeben.

    "=======================================================================
    " TYPES
    "=======================================================================
    TYPES:
      BEGIN OF ty_address,
        number        TYPE string,
        name1         TYPE string,
        name2         TYPE string,
        street        TYPE string,
        street_number TYPE string,
        zip_code      TYPE string,
        city          TYPE string,
      END OF ty_address,

      BEGIN OF ty_position,
        sort_number      TYPE i,
        item_number      TYPE string,
        item_description TYPE string,
        quantity         TYPE decfloat16,
        unit             TYPE string,
        item_price       TYPE decfloat16,
        position_type    TYPE string,
      END OF ty_position,
      tt_positions TYPE STANDARD TABLE OF ty_position WITH EMPTY KEY,

      BEGIN OF ty_container,
        quantity              TYPE i,
        movement_type         TYPE string,
        container_type_name   TYPE string,
        container_type_number TYPE string,
        customer_owned        TYPE abap_bool,
      END OF ty_container,
      tt_containers TYPE STANDARD TABLE OF ty_container WITH EMPTY KEY,

      BEGIN OF ty_bms_order,
        status                     TYPE string,
        order_number               TYPE string,
        order_sheet                TYPE string,
        order_sheet_type           TYPE string,
        customer                   TYPE ty_address,
        place_of_delivery          TYPE ty_address,
        location                   TYPE ty_address,
        estimated_duration         TYPE i,
        planned_date               TYPE string,
        execution_date             TYPE string,
        execution_time_frame_start TYPE string,
        execution_time_frame_end   TYPE string,
        execution_time             TYPE string,
        notes                      TYPE string,
        special_notes              TYPE string,
        producer                   TYPE ty_address,
        recycler                   TYPE ty_address,
        carrier                    TYPE ty_address,
        garbage_key                TYPE string,
        garbage_name               TYPE string,
        " Serialized as collectiveConsignmentNoteNumber via REPLACE in Step 7
        coll_consignment_note_nr   TYPE string,
        team                       TYPE string,
        contract_related           TYPE abap_bool,
        signature_required         TYPE abap_bool,
        positions                  TYPE tt_positions,
        containers                 TYPE tt_containers,
      END OF ty_bms_order,

      BEGIN OF ty_ewaobj,
        pobjnr         TYPE j_objnr,
        smaufnr        TYPE aufnr,
        ordertxt       TYPE text255,
        servloc        TYPE servloc,
        kunnr          TYPE kunnr,
        kunwe          TYPE kunwe,
        beh_type_new   TYPE behtypneu,
        beh_anzahl     TYPE cont_count,
        order_type     TYPE eorder_type,
        transporter    TYPE ehs_partner,
        disposer       TYPE ehs_partner,
        intappno       TYPE ewa_ehs_appnoint,
        appno          TYPE ewa_ehs_appno,
        planned_time   TYPE pldtim,
        planned_durt   TYPE servdur,
        old_order_date TYPE eorder_date,
        zz_order_date  TYPE dats,
        watp_avvcode   TYPE /watp/davvcode,
        watp_notenr    TYPE /watp/dnotenr,
        watp_noteintnr TYPE /watp/dnoteintnr,
      END OF ty_ewaobj,
      tt_ewaobj TYPE HASHED TABLE OF ty_ewaobj
                WITH UNIQUE KEY pobjnr,

      " Slim projection of /plce/tpdsrv
      " SELECT on base table used because READ ENTITIES of a foreign
      " root entity is forbidden inside the Tour behavior handler
      BEGIN OF ty_service,
        service_uuid              TYPE /plce/pdservice_uuid,
        reference_id              TYPE char30,
        reference_internal_id     TYPE char30,
        action                    TYPE /plce/pdaction,
        requested_date            TYPE /plce/date,
        earliest_date             TYPE /plce/date,
        latest_date               TYPE /plce/date,
        service_window_start_time TYPE /plce/time,
        service_window_end_time   TYPE /plce/time,
        service_window            TYPE /plce/pdservice_window,
        additional_text           TYPE /plce/pdadditional_text,
        functional_location       TYPE char30,
      END OF ty_service,
      tt_services TYPE HASHED TABLE OF ty_service
                  WITH UNIQUE KEY service_uuid,

      BEGIN OF ty_bp_addr,
        partner    TYPE bu_partner,
        name1      TYPE bu_nameor1,
        name2      TYPE bu_nameor2,
        street     TYPE ad_street,
        house_num1 TYPE ad_hsnm1,
        post_code1 TYPE ad_pstcd1,
        city1      TYPE ad_city1,
      END OF ty_bp_addr,
      tt_bp_addr TYPE HASHED TABLE OF ty_bp_addr
                 WITH UNIQUE KEY partner.

    " BMS connection constants
    " In production move credentials to a Z-config table or SM59 destination
    CONSTANTS:
      lc_bms_base_url TYPE string
        VALUE 'http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest',
      lc_bms_user     TYPE string VALUE 'BmsWebApi',
      lc_bms_password TYPE string VALUE 'uh}O1Nm#&lV+2LeS'.

    DATA ls_empty_addr TYPE ty_address.

    "=======================================================================
    " STEP 1 — Read the tours the dispatcher selected in the cockpit
    "=======================================================================
    READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour
        FIELDS ( TourId TourTemplate TourStatus StartDate )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_tours)
      FAILED DATA(lt_failed).

    CHECK lt_failed IS INITIAL.

    "=======================================================================
    " STEP 2 — Read service assignments for those tours
    "           Link table only: gives ServiceUUID + sequence order.
    "           Removed assignments are dropped immediately.
    "=======================================================================
    READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY Tour BY \_ServiceAssignments
        FIELDS ( TourUUID ServiceUUID TourSequence Removed )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_asgmts).

    DELETE lt_asgmts WHERE Removed IS NOT INITIAL.
    SORT lt_asgmts BY TourUUID ASCENDING TourSequence ASCENDING.

    "=======================================================================
    " STEP 3 — Read service data directly from base table /plce/tpdsrv
    "           READ ENTITIES OF /PLCE/R_PDService is not allowed here
    "           because this handler belongs to the Tour entity.
    "=======================================================================
    DATA lt_svc_uuids TYPE RANGE OF /plce/pdservice_uuid.
    lt_svc_uuids = VALUE #(
      FOR ls IN lt_asgmts
      ( sign   = 'I'
        option = 'EQ'
        low    = ls-ServiceUUID ) ).

    DATA lt_services TYPE tt_services.

    IF lt_svc_uuids IS NOT INITIAL.
      SELECT service_uuid,
             reference_id,
             reference_int_id     AS reference_internal_id,
             action,
             requested_date,
             earliest_date,
             latest_date,
             service_window_start AS service_window_start_time,
             service_window_end   AS service_window_end_time,
             service_window,
             additional_text,
             functional_location
        FROM /plce/tpdsrv
        WHERE service_uuid IN @lt_svc_uuids
        INTO CORRESPONDING FIELDS OF TABLE @lt_services.
    ENDIF.

    "=======================================================================
    " STEP 4 — Bulk SELECT from EWA_ORDER_OBJECT
    "           Key: POBJNR = reference_internal_id from service
    "=======================================================================
    DATA lt_pobjnr TYPE RANGE OF j_objnr.
    lt_pobjnr = VALUE #(
      FOR lss IN lt_services
      WHERE ( reference_internal_id IS NOT INITIAL )
      ( sign   = 'I'
        option = 'EQ'
        low    = lss-reference_internal_id ) ).

    DATA lt_ewaobj TYPE tt_ewaobj.

    IF lt_pobjnr IS NOT INITIAL.
      SELECT pobjnr,
             smaufnr,
             ordertxt,
             servloc,
             kunnr,
             kunwe,
             beh_type_new,
             beh_anzahl,
             order_type,
             transporter,
             disposer,
             intappno,
             appno,
             planned_time,
             planned_durt,
             old_order_date,
             zz_order_date,
             /watp/avvcode   AS watp_avvcode,
             /watp/notenr    AS watp_notenr,
             /watp/noteintnr AS watp_noteintnr
        FROM ewa_order_object
        WHERE pobjnr IN @lt_pobjnr
        INTO CORRESPONDING FIELDS OF TABLE @lt_ewaobj.
    ENDIF.

    "=======================================================================
    " STEP 5 — Bulk BP address lookup
    "           Customer (KUNNR), goods recipient (KUNWE),
    "           carrier (TRANSPORTER), recycler (DISPOSER)
    "=======================================================================
    DATA lt_bp_range TYPE RANGE OF bu_partner.

    LOOP AT lt_ewaobj INTO DATA(ls_e).
      INSERT VALUE #( sign = 'I' option = 'EQ' low = ls_e-kunnr )
        INTO TABLE lt_bp_range.
      INSERT VALUE #( sign = 'I' option = 'EQ' low = ls_e-kunwe )
        INTO TABLE lt_bp_range.
      INSERT VALUE #( sign = 'I' option = 'EQ' low = ls_e-transporter )
        INTO TABLE lt_bp_range.
      INSERT VALUE #( sign = 'I' option = 'EQ' low = ls_e-disposer )
        INTO TABLE lt_bp_range.
    ENDLOOP.
    DELETE ADJACENT DUPLICATES FROM lt_bp_range.

    DATA lt_bp_addr TYPE tt_bp_addr.

    IF lt_bp_range IS NOT INITIAL.
      SELECT bp~partner,
             bp~bu_sort1  AS name1,
             bp~bu_sort2  AS name2,
             adr~street,
             adr~house_num1,
             adr~post_code1,
             adr~city1
        FROM but000 AS bp
        INNER JOIN but020 AS ba  ON  ba~partner     = bp~partner
        INNER JOIN adrc   AS adr ON  adr~addrnumber = ba~addrnumber
                                  AND adr~nation     = ba~nation
        WHERE bp~partner IN @lt_bp_range
          AND ba~date_from <= @sy-datum
        INTO CORRESPONDING FIELDS OF TABLE @lt_bp_addr.
    ENDIF.

    "=======================================================================
    " STEP 6 — Main loop: one Bearer token per tour, then POST per service
    "           PERFORMANCE FIX: get_bms_bearer_token is called ONCE per
    "           tour and reused across all services on that tour.
    "           Previously it was called inside the inner loop (once per
    "           service) causing N redundant auth round-trips per tour.
    "=======================================================================
    LOOP AT lt_tours INTO DATA(ls_tour).

      "-----------------------------------------------------------------------
      " 6a  Kolonne lookup: tour template → BMS team name
      "-----------------------------------------------------------------------
      SELECT SINGLE bms_team
        FROM ztour_bms_kolonn
        WHERE tour_template = @ls_tour-TourTemplate
        INTO @DATA(lv_team).

      IF sy-subrc <> 0.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '014'
                   severity = if_abap_behv_message=>severity-warning
                   v1       = ls_tour-TourTemplate
                   v2       = ls_tour-TourId )
        ) TO reported-tour.
        CLEAR lv_team.
      ENDIF.

      "-----------------------------------------------------------------------
      " 6b  Authenticate once per tour — token reused for every service
      "     on this tour. Tokens are valid for minutes so this is safe.
      "-----------------------------------------------------------------------
      DATA lv_bearer_token TYPE string.
      DATA lv_auth_error   TYPE string.
      CLEAR: lv_bearer_token, lv_auth_error.

      get_bms_bearer_token(
        EXPORTING
          iv_base_url = lc_bms_base_url
          iv_username = lc_bms_user
          iv_password = lc_bms_password
        IMPORTING
          ev_token    = lv_bearer_token
          ev_error    = lv_auth_error ).

      IF lv_auth_error IS NOT INITIAL.
        " Auth failed for this tour — mark error, skip all its services
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message_with_text(
                   severity = if_abap_behv_message=>severity-error
                   text     = lv_auth_error )
        ) TO reported-tour.

        " Write ERROR status immediately and continue to next tour
        MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY ExtCustom
            UPDATE FIELDS ( zz_bms_status )
            WITH VALUE #(
              ( TourUUID      = ls_tour-TourUUID
                zz_bms_status = 'ERROR' ) )
          FAILED   DATA(lf_auth_mod)
          REPORTED DATA(lr_auth_mod).

        IF lf_auth_mod IS NOT INITIAL.
          MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
            ENTITY Tour
              CREATE BY \_ExtCustom
                FIELDS ( zz_bms_status )
                WITH VALUE #(
                  ( %tky    = ls_tour-%tky
                    %target = VALUE #(
                      ( zz_bms_status = 'ERROR' ) ) ) )
            FAILED   DATA(lf_auth_crt)
            REPORTED DATA(lr_auth_crt).
        ENDIF.

        CONTINUE.
      ENDIF.

      DATA lv_tour_has_error TYPE abap_bool VALUE abap_false.

      LOOP AT lt_asgmts INTO DATA(ls_asgmt)
        WHERE TourUUID = ls_tour-TourUUID.

        " Service data for this assignment
        DATA ls_svc TYPE ty_service.
        CLEAR ls_svc.
        READ TABLE lt_services INTO ls_svc
          WITH KEY service_uuid = ls_asgmt-ServiceUUID.
        CHECK sy-subrc = 0.

        " Corresponding EWA order row — POBJNR = reference_internal_id
        DATA ls_ewa TYPE ty_ewaobj.
        CLEAR ls_ewa.
        READ TABLE lt_ewaobj INTO ls_ewa
          WITH KEY pobjnr = ls_svc-reference_internal_id.

        IF sy-subrc <> 0.
          lv_tour_has_error = abap_true.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '012'
                     severity = if_abap_behv_message=>severity-warning
                     v1       = ls_svc-reference_id )
          ) TO reported-tour.
          CONTINUE.
        ENDIF.

        "---------------------------------------------------------------------
        " 6c  Movement type: PROLOGA action → BMS movementType
        "     01 Stellen→S  02 Holen→H  03 Tauschen→T
        "     04 Ersetzen→T  05 Umleeren→T
        "---------------------------------------------------------------------
        DATA(lv_movement_type) = SWITCH string( ls_svc-action
          WHEN '01' THEN 'S'
          WHEN '02' THEN 'H'
          WHEN '03' THEN 'T'
          WHEN '04' THEN 'T'
          WHEN '05' THEN 'T'
          ELSE           'S' ).

        IF NOT ls_svc-action CA '12345'.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '013'
                     severity = if_abap_behv_message=>severity-warning
                     v1       = ls_svc-action
                     v2       = ls_svc-reference_id )
          ) TO reported-tour.
        ENDIF.

        DATA(lv_is_express) = xsdbool( ls_ewa-order_type = '02' ).

        "---------------------------------------------------------------------
        " 6d  Container array — one row from EWA flat fields
        "---------------------------------------------------------------------
        DATA lt_containers TYPE tt_containers.
        CLEAR lt_containers.
        IF ls_ewa-beh_type_new IS NOT INITIAL.
          APPEND VALUE #(
            quantity              = ls_ewa-beh_anzahl
            movement_type         = lv_movement_type
            container_type_name   = ls_ewa-beh_type_new
            container_type_number = ls_ewa-beh_type_new
            customer_owned        = abap_false
          ) TO lt_containers.
        ENDIF.

        "---------------------------------------------------------------------
        " 6e  Positions from SD sales order items (VBAP)
        "---------------------------------------------------------------------
        DATA lt_positions TYPE tt_positions.
        CLEAR lt_positions.
        IF ls_svc-reference_id IS NOT INITIAL.
          SELECT vbeln, posnr, matnr, arktx, kwmeng, meins, netpr
            FROM vbap
            WHERE vbeln = @ls_svc-reference_id
            INTO TABLE @DATA(lt_vbap).

          lt_positions = VALUE #( FOR ls_p IN lt_vbap
            ( sort_number      = ls_p-posnr
              item_number      = ls_p-matnr
              item_description = ls_p-arktx
              quantity         = ls_p-kwmeng
              unit             = ls_p-meins
              item_price       = ls_p-netpr
              position_type    = 'P' ) ).
        ENDIF.

        "---------------------------------------------------------------------
        " 6f  LEISTUNGSZEIT — time window with EWA fallback
        "---------------------------------------------------------------------
        DATA(lv_exec_time) = COND string(
          WHEN ls_svc-service_window_start_time IS NOT INITIAL
           AND ls_svc-service_window_end_time   IS NOT INITIAL
           AND ls_svc-service_window_start_time <> ls_svc-service_window_end_time
          THEN |{ ls_svc-service_window_start_time TIME = USER } bis | &&
               |{ ls_svc-service_window_end_time   TIME = USER }|
          WHEN ls_svc-service_window_start_time IS NOT INITIAL
           AND ls_svc-service_window_end_time   IS NOT INITIAL
          THEN |{ ls_svc-service_window_start_time TIME = USER }|
          WHEN ls_svc-service_window_start_time IS NOT INITIAL
          THEN |ab { ls_svc-service_window_start_time TIME = USER }|
          WHEN ls_svc-service_window_end_time IS NOT INITIAL
          THEN |bis { ls_svc-service_window_end_time TIME = USER }|
          WHEN ls_ewa-planned_time IS NOT INITIAL
          THEN |{ ls_ewa-planned_time TIME = USER }|
          ELSE CONV string( ls_svc-service_window ) ).

        "---------------------------------------------------------------------
        " 6g  signatureRequired — scan notes for UTERSCHR
        "---------------------------------------------------------------------
        DATA(lv_sig_req) = xsdbool(
          to_upper( ls_ewa-ordertxt )        CS 'UTERSCHR' OR
          to_upper( ls_svc-additional_text ) CS 'UTERSCHR' ).

        "---------------------------------------------------------------------
        " 6h  Notes enrichment per old interface doc rules
        "---------------------------------------------------------------------
        DATA(lv_notes) = CONV string( ls_ewa-ordertxt ).

        IF ls_ewa-smaufnr  CS 'IS'  OR ls_ewa-kunwe CS 'IS'
        OR ls_ewa-smaufnr  CS 'RKT' OR ls_ewa-kunwe CS 'RKT'
        OR ls_ewa-kunnr = '0000113840'
        OR ls_ewa-kunnr = '0000125669'.
          lv_notes &&= ' AUFTRAG ZU KVV'.
        ENDIF.

        IF ls_ewa-smaufnr IS NOT INITIAL.
          lv_notes &&= | Bestellnummer: { ls_ewa-smaufnr ALPHA = OUT }|.
        ENDIF.

        IF lv_is_express = abap_true.
          lv_notes &&= ' EXPRESSAUFTRAG'.
        ENDIF.

        DATA(lv_cert_nr) = COND string(
          WHEN ls_ewa-appno    IS NOT INITIAL THEN ls_ewa-appno
          WHEN ls_ewa-intappno IS NOT INITIAL THEN ls_ewa-intappno ).
        IF lv_cert_nr IS NOT INITIAL.
          lv_notes &&= | Entsorgungsnachweis: { lv_cert_nr }|.
        ENDIF.

        "---------------------------------------------------------------------
        " 6i  BP address blocks
        "---------------------------------------------------------------------
        DATA ls_cust_bp     TYPE ty_bp_addr.
        DATA ls_kunwe_bp    TYPE ty_bp_addr.
        DATA ls_carrier_bp  TYPE ty_bp_addr.
        DATA ls_recycler_bp TYPE ty_bp_addr.
        CLEAR: ls_cust_bp, ls_kunwe_bp, ls_carrier_bp, ls_recycler_bp.

        READ TABLE lt_bp_addr INTO ls_cust_bp
          WITH KEY partner = ls_ewa-kunnr.
        READ TABLE lt_bp_addr INTO ls_kunwe_bp
          WITH KEY partner = ls_ewa-kunwe.
        READ TABLE lt_bp_addr INTO ls_carrier_bp
          WITH KEY partner = ls_ewa-transporter.
        READ TABLE lt_bp_addr INTO ls_recycler_bp
          WITH KEY partner = ls_ewa-disposer.

        "---------------------------------------------------------------------
        " 6j  Material description for garbageName (FIELD SWAP per doc)
        "---------------------------------------------------------------------
        DATA lv_material_desc TYPE maktx.
        CLEAR lv_material_desc.
        IF ls_ewa-watp_avvcode IS NOT INITIAL.
          SELECT SINGLE maktx
            FROM makt
            WHERE matnr = @ls_ewa-watp_avvcode
              AND spras = @sy-langu
            INTO @lv_material_desc.
        ENDIF.

        "---------------------------------------------------------------------
        " 6k  Duration: PLANNED_DURT (HHMMSS) → integer minutes
        "---------------------------------------------------------------------
        DATA(lv_duration_min) = CONV i(
          ls_ewa-planned_durt(2) * 60 +
          ls_ewa-planned_durt+2(2) ).

        "---------------------------------------------------------------------
        " 6l  Assemble the BMS payload
        "---------------------------------------------------------------------
        DATA(ls_order) = VALUE ty_bms_order(
          status           = 'OK'
          order_number     = |{ ls_ewa-smaufnr ALPHA = OUT }|
          order_sheet      = ls_ewa-watp_notenr
          order_sheet_type = 'LS'

          customer = VALUE #(
            number        = |{ ls_ewa-kunnr ALPHA = OUT }|
            name1         = ls_cust_bp-name1
            name2         = ls_cust_bp-name2
            street        = ls_cust_bp-street
            street_number = ls_cust_bp-house_num1
            zip_code      = ls_cust_bp-post_code1
            city          = ls_cust_bp-city1 )

          place_of_delivery = VALUE #(
            number        = |{ ls_ewa-kunwe ALPHA = OUT }|
            name1         = ls_kunwe_bp-name1
            name2         = ls_kunwe_bp-name2
            street        = ls_kunwe_bp-street
            street_number = ls_kunwe_bp-house_num1
            zip_code      = ls_kunwe_bp-post_code1
            city          = ls_kunwe_bp-city1 )

          location = VALUE #(
            street = ls_ewa-servloc )

          estimated_duration = lv_duration_min

          planned_date = COND string(
            WHEN ls_svc-earliest_date IS NOT INITIAL
            THEN |{ ls_svc-earliest_date DATE = ISO }T00:00:00.000Z|
            ELSE |{ ls_ewa-zz_order_date DATE = ISO }T00:00:00.000Z| )

          execution_date = COND string(
            WHEN ls_svc-requested_date IS NOT INITIAL
            THEN |{ ls_svc-requested_date DATE = ISO }T00:00:00.000Z|
            ELSE |{ ls_ewa-old_order_date DATE = ISO }T00:00:00.000Z| )

          execution_time_frame_start = COND string(
            WHEN ls_svc-service_window_start_time IS NOT INITIAL
            THEN |{ ls_svc-service_window_start_time TIME = USER }| )

          execution_time_frame_end = COND string(
            WHEN ls_svc-service_window_end_time IS NOT INITIAL
            THEN |{ ls_svc-service_window_end_time TIME = USER }| )

          execution_time = lv_exec_time
          notes          = lv_notes

          special_notes = COND string(
            WHEN lv_is_express = abap_true
            THEN |EXPRESSAUFTRAG| &&
                 COND #( WHEN ls_svc-additional_text IS NOT INITIAL
                          THEN | - | && ls_svc-additional_text )
            ELSE ls_svc-additional_text )

          producer = ls_empty_addr

          recycler = VALUE #(
            number        = |{ ls_ewa-disposer ALPHA = OUT }|
            name1         = ls_recycler_bp-name1
            name2         = ls_recycler_bp-name2
            street        = ls_recycler_bp-street
            street_number = ls_recycler_bp-house_num1
            zip_code      = ls_recycler_bp-post_code1
            city          = ls_recycler_bp-city1 )

          carrier = VALUE #(
            number        = |{ ls_ewa-transporter ALPHA = OUT }|
            name1         = ls_carrier_bp-name1
            name2         = ls_carrier_bp-name2
            street        = ls_carrier_bp-street
            street_number = ls_carrier_bp-house_num1
            zip_code      = ls_carrier_bp-post_code1
            city          = ls_carrier_bp-city1 )

          garbage_key  = ls_ewa-watp_avvcode
          garbage_name = lv_material_desc

          coll_consignment_note_nr =
            COND #( WHEN ls_ewa-watp_noteintnr IS NOT INITIAL
                    THEN ls_ewa-watp_noteintnr )

          team               = lv_team
          contract_related   = abap_false
          signature_required = lv_sig_req
          positions          = lt_positions
          containers         = lt_containers ).

        "=====================================================================
        " STEP 7 — Serialize to JSON + patch the long field name
        "=====================================================================
        DATA(lv_json) = /ui2/cl_json=>serialize(
          data        = ls_order
          pretty_name = /ui2/cl_json=>pretty_mode-camel_case ).

        REPLACE ALL OCCURRENCES OF '"collConsignmentNoteNr"'
          IN lv_json
          WITH '"collectiveConsignmentNoteNumber"'.

        "=====================================================================
        " STEP 8 — POST via helper method using the tour-level Bearer token
        "           Token was obtained once above (Step 6b) and is reused
        "           for every service on this tour — no repeated auth calls
        "=====================================================================
        DATA lv_http_status TYPE i.
        DATA lv_response    TYPE string.
        CLEAR: lv_http_status, lv_response.

        post_bms_order(
          EXPORTING
            iv_base_url     = lc_bms_base_url
            iv_bearer_token = lv_bearer_token
            iv_json         = lv_json
          IMPORTING
            ev_http_status  = lv_http_status
            ev_response     = lv_response ).

        "=====================================================================
        " STEP 9 — Evaluate HTTP response and report back to the cockpit UI
        "=====================================================================
        IF lv_http_status = 201.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '010'
                     severity = if_abap_behv_message=>severity-success
                     v1       = |{ ls_ewa-smaufnr ALPHA = OUT }| )
          ) TO reported-tour.
        ELSE.
          lv_tour_has_error = abap_true.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '011'
                     severity = if_abap_behv_message=>severity-error
                     v1       = lv_http_status
                     v2       = lv_response )
          ) TO reported-tour.
        ENDIF.

      ENDLOOP. " lt_asgmts WHERE TourUUID

      "=======================================================================
      " STEP 10 — Write ZZ_BMS_STATUS back to the tour extension
      "           'SENT'  → green tick in BMS-Status column
      "           'ERROR' → red cross in BMS-Status column
      "=======================================================================
      DATA(lv_bms_status) = COND /plce/char20(
        WHEN lv_tour_has_error = abap_false THEN 'SENT'
        ELSE                                     'ERROR' ).

      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY ExtCustom
          UPDATE FIELDS ( zz_bms_status )
          WITH VALUE #(
            ( TourUUID      = ls_tour-TourUUID
              zz_bms_status = lv_bms_status ) )
        FAILED   DATA(lf_mod)
        REPORTED DATA(lr_mod).

      IF lf_mod IS NOT INITIAL.
        MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour
            CREATE BY \_ExtCustom
              FIELDS ( zz_bms_status )
              WITH VALUE #(
                ( %tky    = ls_tour-%tky
                  %target = VALUE #(
                    ( zz_bms_status = lv_bms_status ) ) ) )
          FAILED   DATA(lf_crt)
          REPORTED DATA(lr_crt).
      ENDIF.

    ENDLOOP. " lt_tours

  ENDMETHOD.

ENDCLASS.
