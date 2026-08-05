METHOD touranbmsfreigeben.

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
        container_number_old  TYPE string,
        container_number_new  TYPE string,
        customer_owned        TYPE abap_bool,
      END OF ty_container,
      tt_containers TYPE STANDARD TABLE OF ty_container WITH EMPTY KEY,

      BEGIN OF ty_bms_order,
        status                     TYPE string,
        order_number               TYPE string,
        order                      TYPE string,
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
        coll_consignment_note_nr   TYPE string,
        team                       TYPE string,
        contract_related           TYPE abap_bool,
        signature_required         TYPE abap_bool,
        positions                  TYPE tt_positions,
        containers                 TYPE tt_containers,
      END OF ty_bms_order,

      BEGIN OF ty_ewaobj,
        pobjnr         TYPE j_objnr,
        ordernr        TYPE eordernr,
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
        sdaufnr        TYPE vbeln_va,   " SD sales order for this service position
        sdposnr        TYPE posnr,      " SD sales order position for this service
      END OF ty_ewaobj,
      tt_ewaobj TYPE HASHED TABLE OF ty_ewaobj
                WITH UNIQUE KEY pobjnr,

      BEGIN OF ty_service,
        service_uuid              TYPE /plce/pdservice_uuid,
        service_id                type /PLCE/PDSERVICE_ID,
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
        container_atloc_tidnr     TYPE char30,   " TIDNR at location → containerNumberOld
        container_new_tidnr       TYPE char30,   " TIDNR new container → containerNumberNew
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

    "=======================================================================
    " CONFIG — read from ZTOUR_BMS_CFG (replaces hardcoded constants)
    "          default_carrier and default_recycler are BP numbers used
    "          when transporter/disposer are not populated on EWA record
    "=======================================================================
    SELECT SINGLE bms_endpoint_url,
                  bms_username,
                  bms_password,
                  active,
                  default_carrier,
                  default_recycler
      FROM ztour_bms_cfg
      WHERE config_id = 'DEFAULT'
      INTO @DATA(ls_cfg).

    IF sy-subrc <> 0 OR ls_cfg-active <> 'X'.
      LOOP AT keys ASSIGNING FIELD-SYMBOL(<ky>).
        APPEND VALUE #(
          %tky = <ky>-%tky
          %msg = new_message_with_text(
                   severity = if_abap_behv_message=>severity-information
                   text     = 'BMS interface not configured or inactive — maintain ZTOUR_BMS_CFG' )
        ) TO reported-tour.
      ENDLOOP.
      RETURN.
    ENDIF.

    DATA(lv_bms_base_url) = ls_cfg-bms_endpoint_url.
    DATA(lv_bms_user)     = ls_cfg-bms_username.
    DATA(lv_bms_password) = ls_cfg-bms_password.
    DATA(lv_def_carrier)  = ls_cfg-default_carrier.
    DATA(lv_def_recycler) = ls_cfg-default_recycler.

    "=======================================================================
    " STEP 1 — Read the tours the dispatcher selected in the cockpit
    "=======================================================================
    READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY tour
        FIELDS ( tourid tourtemplate tourstatus startdate )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_tours)
      FAILED DATA(lt_failed).

    CHECK lt_failed IS INITIAL.

    "=======================================================================
    " STEP 2 — Read service assignments (link table: UUID + sequence)
    "=======================================================================
    READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY tour BY \_serviceassignments
        FIELDS ( touruuid serviceuuid toursequence removed )
        WITH CORRESPONDING #( keys )
      RESULT DATA(lt_asgmts).

    DELETE lt_asgmts WHERE removed IS NOT INITIAL.
    SORT lt_asgmts BY touruuid ASCENDING toursequence ASCENDING.

    "=======================================================================
    " STEP 3 — Read service data from /plce/tpdsrv + /plce/tpdsrvwr
    "           LEFT OUTER JOIN to /plce/tpdsrvwr gives TIDNR fields
    "           for containerNumberOld and containerNumberNew
    "           READ ENTITIES of foreign root is forbidden — direct SELECT
    "=======================================================================
    DATA lt_svc_uuids TYPE RANGE OF /plce/pdservice_uuid.
    lt_svc_uuids = VALUE #(
      FOR ls IN lt_asgmts
      ( sign = 'I' option = 'EQ' low = ls-serviceuuid ) ).

    DATA lt_services TYPE tt_services.

    IF lt_svc_uuids IS NOT INITIAL.
      SELECT s~service_uuid,
             s~reference_id,
             s~service_id,
             s~reference_int_id     AS reference_internal_id,
             s~action,
             s~requested_date,
             s~earliest_date,
             s~latest_date,
             s~service_window_start AS service_window_start_time,
             s~service_window_end   AS service_window_end_time,
             s~service_window,
             s~additional_text,
             s~functional_location,
             w~container_atloc_tidnr,
             w~container_new_tidnr
        FROM /plce/tpdsrv AS s
        LEFT OUTER JOIN /plce/tpdsrvwr AS w
          ON w~service_uuid = s~service_uuid
        WHERE s~service_uuid IN @lt_svc_uuids
        INTO CORRESPONDING FIELDS OF TABLE @lt_services.
    ENDIF.

    "=======================================================================
    " STEP 4 — Bulk SELECT from EWA_ORDER_OBJECT
    "           sdaufnr + sdposnr identify the exact SD billing position
    "           for this container service — used in step 6e
    "=======================================================================
    DATA lt_pobjnr TYPE RANGE OF j_objnr.
    lt_pobjnr = VALUE #(
      FOR lss IN lt_services
      WHERE ( reference_internal_id IS NOT INITIAL )
      ( sign = 'I' option = 'EQ' low = lss-reference_internal_id ) ).

    DATA lt_ewaobj TYPE tt_ewaobj.

    IF lt_pobjnr IS NOT INITIAL.
      SELECT pobjnr,
             ordernr,
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
             /watp/noteintnr AS watp_noteintnr,
             sdaufnr,
             sdposnr
        FROM ewa_order_object
        WHERE pobjnr IN @lt_pobjnr
        INTO CORRESPONDING FIELDS OF TABLE @lt_ewaobj.
    ENDIF.

    "=======================================================================
    " STEP 5 — Bulk BP address lookup
    "          Includes default_carrier and default_recycler from config
    "          so fallback addresses are pre-loaded in the same SELECT
    "          bu_name1 is used instead of bu_sort1 — sort fields are often
    "          blank while the name fields are reliably populated
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

    " Add config defaults so their addresses are available without extra SELECTs
    IF lv_def_carrier IS NOT INITIAL.
      INSERT VALUE #( sign = 'I' option = 'EQ' low = lv_def_carrier )
        INTO TABLE lt_bp_range.
    ENDIF.
    IF lv_def_recycler IS NOT INITIAL.
      INSERT VALUE #( sign = 'I' option = 'EQ' low = lv_def_recycler )
        INTO TABLE lt_bp_range.
    ENDIF.

    DELETE ADJACENT DUPLICATES FROM lt_bp_range.

    DATA lt_bp_addr TYPE tt_bp_addr.

    IF lt_bp_range IS NOT INITIAL.
      SELECT *
        FROM but000 AS bp
        INNER JOIN but020 AS ba  ON  ba~partner     = bp~partner
        INNER JOIN adrc   AS adr ON  adr~addrnumber = ba~addrnumber
                                 AND adr~nation      = ba~nation
        WHERE bp~partner IN @lt_bp_range
          AND ba~date_from <= @sy-datum
        INTO CORRESPONDING FIELDS OF TABLE @lt_bp_addr.
    ENDIF.

    "=======================================================================
    " STEP 6 — Main loop: one auth per tour, then POST per service
    "=======================================================================
    LOOP AT lt_tours INTO DATA(ls_tour).

      " 6a Kolonne lookup
      SELECT SINGLE bms_team
        FROM ztour_bms_kolonn
        WHERE tour_template = @ls_tour-tourtemplate
        INTO @DATA(lv_team).

        IF sy-subrc <> 0.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '019'
                     severity = if_abap_behv_message=>severity-information
                     v1       = ls_tour-tourid
                     v2       = ls_tour-tourtemplate )
          ) TO reported-tour.
          CLEAR lv_team.
        ENDIF.

      " 6b Auth once per tour
      DATA lv_bearer_token TYPE string.
      DATA lv_auth_error   TYPE string.
      CLEAR: lv_bearer_token, lv_auth_error.

      zcl_wr_pd_tour_helper=>get_bms_bearer_token(
        EXPORTING
          iv_base_url = lv_bms_base_url
          iv_username = lv_bms_user
          iv_password = lv_bms_password
        IMPORTING
          ev_token    = lv_bearer_token
          ev_error    = lv_auth_error ).

      IF lv_auth_error IS NOT INITIAL.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message_with_text(
                   severity = if_abap_behv_message=>severity-error
                   text     = lv_auth_error )
        ) TO reported-tour.

        MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
          ENTITY extcustom
            UPDATE FIELDS ( zz_bms_status )
            WITH VALUE #( ( touruuid = ls_tour-touruuid zz_bms_status = 'ERROR' ) )
          FAILED   DATA(lf_auth_mod)
          REPORTED DATA(lr_auth_mod).
            IF lf_auth_mod IS NOT INITIAL.
                      MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
                        ENTITY tour
                          CREATE BY \_extcustom
                            FIELDS ( zz_bms_status )
                            WITH VALUE #( (
                                            %tky    = ls_tour-%tky
                                            %target = VALUE #( (
                                              %cid          = |BMS_AUTH_TGT_{ ls_tour-touruuid }|
                                              zz_bms_status = 'ERROR' ) ) ) )
                        FAILED DATA(lf_auth_crt) REPORTED DATA(lr_auth_crt).
                    ENDIF.

        CONTINUE.
      ENDIF.

      DATA lv_tour_has_error TYPE abap_bool VALUE abap_false.

      LOOP AT lt_asgmts INTO DATA(ls_asgmt)
        WHERE touruuid = ls_tour-touruuid.

        DATA ls_svc TYPE ty_service.
        CLEAR ls_svc.
        READ TABLE lt_services INTO ls_svc
          WITH KEY service_uuid = ls_asgmt-serviceuuid.
        CHECK sy-subrc = 0.

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
                     severity = if_abap_behv_message=>severity-information
                     v1       = ls_svc-reference_id )
          ) TO reported-tour.
          CONTINUE.
        ENDIF.

        " 6c Movement type — read from ZWRTWAALAPRCTP
        "    Valid BMS enum values for OptiAwiMovementType must be confirmed
        "    with the BMS team and maintained in ZWRTWAALAPRCTP via SM30
        DATA lv_movement_type TYPE string.

        SELECT SINGLE bms_movement_type
          FROM zwrtwaalaprctp
          WHERE service_type = @ls_svc-action
          INTO @lv_movement_type.

       IF sy-subrc <> 0 OR lv_movement_type IS INITIAL.
          lv_movement_type = 'S'.

          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '020'
                     severity = if_abap_behv_message=>severity-information
                     v1       = condense( |{ ls_svc-reference_id ALPHA = OUT }| )
                     v2       = ls_svc-action
                     v3       = condense( |{ ls_tour-tourid ALPHA = OUT }| )  )
          ) TO reported-tour.
        ENDIF.

        DATA(lv_is_express) = xsdbool( ls_ewa-order_type = '02' ).

        " 6d Container array
        "    container_number_old / container_number_new
        "    mapped from PROLOGA TIDNR fields via /plce/tpdsrvwr JOIN
        "    When beh_type_new is initial the container type is unknown —
        "    a placeholder is sent so BMS min-1-element validation passes.
        "    ContainerTypeName and ContainerTypeNumber use the service action
        "    as a readable placeholder until real data is available.
        "    ContainerNumberNew is set to '0' for movement type S because
        "    BMS requires it and no TIDNR is known in this fallback path.
        DATA lt_containers TYPE tt_containers.
        CLEAR lt_containers.
        IF ls_ewa-beh_type_new IS NOT INITIAL.
          APPEND VALUE #(
            quantity              = ls_ewa-beh_anzahl
            movement_type         = lv_movement_type
            container_type_name   = ls_ewa-beh_type_new
            container_type_number = ls_ewa-beh_type_new
            container_number_old  = ls_svc-container_atloc_tidnr
            container_number_new  = ls_svc-container_new_tidnr
            customer_owned        = abap_false
          ) TO lt_containers.
        ELSEIF lv_movement_type IS NOT INITIAL.
          " Minimal fallback — container type not known from EWA record
          APPEND VALUE #(
            quantity              = 1
            movement_type         = lv_movement_type
            container_type_name   = condense( ls_svc-action )
            container_type_number = condense( ls_svc-action )
            container_number_old  = condense( ls_svc-container_atloc_tidnr )
            container_number_new  = COND #(
                                      WHEN ls_svc-container_new_tidnr IS NOT INITIAL
                                      THEN condense( ls_svc-container_new_tidnr )
                                      ELSE '0' )
            customer_owned        = abap_false
          ) TO lt_containers.
        ENDIF.

        " 6e Positions from VBAP
        "    Uses EWA_ORDER_OBJECT.sdaufnr + sdposnr to read the single
        "    SD billing position for this specific container service.
        "    Positions array is currently kept empty because BMS has not
        "    confirmed the valid enum values for OptiAwiPositionType.
        "    Re-enable once positionType mapping is agreed with BMS team.
        DATA lt_positions TYPE tt_positions.
        CLEAR lt_positions.

        " 6f Leistungszeit
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

        " 6g Signature check
        DATA(lv_sig_req) = xsdbool(
          to_upper( ls_ewa-ordertxt )        CS 'UTERSCHR' OR
          to_upper( ls_svc-additional_text ) CS 'UTERSCHR' ).

        " 6h Notes enrichment
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

        " 6i BP addresses
        "    Carrier: use transporter from EWA if populated, else default
        "             from ZTOUR_BMS_CFG.default_carrier
        "    Recycler: use disposer from EWA if populated, else default
        "              from ZTOUR_BMS_CFG.default_recycler
        DATA ls_cust_bp     TYPE ty_bp_addr.
        DATA ls_kunwe_bp    TYPE ty_bp_addr.
        DATA ls_carrier_bp  TYPE ty_bp_addr.
        DATA ls_recycler_bp TYPE ty_bp_addr.
        CLEAR: ls_cust_bp, ls_kunwe_bp, ls_carrier_bp, ls_recycler_bp.

        READ TABLE lt_bp_addr INTO ls_cust_bp  WITH KEY partner = ls_ewa-kunnr.
        READ TABLE lt_bp_addr INTO ls_kunwe_bp WITH KEY partner = ls_ewa-kunwe.

        IF ls_ewa-transporter IS NOT INITIAL.
          READ TABLE lt_bp_addr INTO ls_carrier_bp
            WITH KEY partner = ls_ewa-transporter.
        ENDIF.
        IF ls_carrier_bp IS INITIAL AND lv_def_carrier IS NOT INITIAL.
          READ TABLE lt_bp_addr INTO ls_carrier_bp
            WITH KEY partner = lv_def_carrier.
        ENDIF.

        IF ls_ewa-disposer IS NOT INITIAL.
          READ TABLE lt_bp_addr INTO ls_recycler_bp
            WITH KEY partner = ls_ewa-disposer.
        ENDIF.
        IF ls_recycler_bp IS INITIAL AND lv_def_recycler IS NOT INITIAL.
          READ TABLE lt_bp_addr INTO ls_recycler_bp
            WITH KEY partner = lv_def_recycler.
        ENDIF.

        " 6j Material description
        DATA lv_material_desc TYPE maktx.
        CLEAR lv_material_desc.
        IF ls_ewa-watp_avvcode IS NOT INITIAL.
          SELECT SINGLE maktx
            FROM makt
            WHERE matnr = @ls_ewa-watp_avvcode
              AND spras = @sy-langu
            INTO @lv_material_desc.
        ENDIF.

        " 6k Duration: HHMMSS → minutes
        DATA(lv_duration_min) = CONV i(
          ls_ewa-planned_durt(2) * 60 +
          ls_ewa-planned_durt+2(2) ).

        " 6l Order reference for BMS 'order' field
        "    Apply ALPHA = OUT + condense to each candidate so that fields
        "    containing only leading zeros collapse to empty and the next
        "    candidate in the chain is tried
        DATA(lv_ordernr_clean)   = condense( |{ ls_ewa-ordernr ALPHA = OUT }| ).
        DATA(lv_sdaufnr_clean)   = condense( |{ ls_ewa-sdaufnr ALPHA = OUT }| ).
        DATA(lv_smaufnr_clean)   = condense( |{ ls_ewa-smaufnr ALPHA = OUT }| ).

        DATA(lv_order_ref) = COND string(
          WHEN lv_ordernr_clean IS NOT INITIAL THEN lv_ordernr_clean
          WHEN lv_sdaufnr_clean IS NOT INITIAL THEN lv_sdaufnr_clean
          WHEN lv_smaufnr_clean IS NOT INITIAL THEN lv_smaufnr_clean
