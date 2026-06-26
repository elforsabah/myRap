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
                   severity = if_abap_behv_message=>severity-error
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
      SELECT bp~partner,
             bp~bu_name1  AS name1,
             bp~bu_name2  AS name2,
             adr~street,
             adr~house_num1,
             adr~post_code1,
             adr~city1
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
                   number   = '014'
                   severity = if_abap_behv_message=>severity-warning
                   v1       = ls_tour-tourtemplate
                   v2       = ls_tour-tourid )
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
                WITH VALUE #( ( %tky = ls_tour-%tky
                                %target = VALUE #( ( zz_bms_status = 'ERROR' ) ) ) )
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
                     severity = if_abap_behv_message=>severity-warning
                     v1       = ls_svc-reference_id )
          ) TO reported-tour.
          CONTINUE.
        ENDIF.

        " 6c Movement type — read from ZWRTWAALAPRCTP
        DATA lv_movement_type TYPE string.

        SELECT SINGLE bms_movement_type
          FROM zwrtwaalaprctp
          WHERE service_type = @ls_svc-action
          INTO @lv_movement_type.

        IF sy-subrc <> 0 OR lv_movement_type IS INITIAL.
          " Fallback to S (Stellen) if not configured
          lv_movement_type = 'S'.
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

        " 6d Container array
        "    container_number_old / container_number_new
        "    mapped from PROLOGA TIDNR fields via /plce/tpdsrvwr JOIN
        "    If no container type is known a minimal fallback entry is sent
        "    using movement type A (Abholen) which does not require
        "    ContainerNumberNew — keeping BMS min-1-element validation happy
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
        ELSE.
          " Minimal fallback — movement type A does not require ContainerNumberNew
          APPEND VALUE #(
            quantity       = 1
            movement_type  = 'A'
            customer_owned = abap_false
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
        "    EWA internal order number preferred; SD contract number as
        "    fallback if ordernr is not populated; smaufnr as last resort
        DATA(lv_order_ref) = COND string(
          WHEN ls_ewa-ordernr IS NOT INITIAL THEN condense( ls_ewa-ordernr )
          WHEN ls_ewa-sdaufnr IS NOT INITIAL THEN |{ ls_ewa-sdaufnr ALPHA = OUT }|
          ELSE                                    |{ ls_ewa-smaufnr ALPHA = OUT }| ).

        " 6m OrderNumber — smaufnr with ALPHA conversion and trailing space trim
        "    Falls back to lv_order_ref when smaufnr is initial to avoid
        "    sending a blank string that BMS rejects as required field
        DATA(lv_order_number) = COND string(
          WHEN condense( ls_ewa-smaufnr ) IS NOT INITIAL
          THEN |{ ls_ewa-smaufnr ALPHA = OUT }|
          ELSE lv_order_ref ).

        " 6n Helper macro for trimmed BP number output
        "    ALPHA = OUT removes leading zeros; condense removes trailing spaces
        "    that CHAR-typed partner fields carry when shorter than field length
        DATA(lv_kunnr_out)       = condense( |{ ls_ewa-kunnr       ALPHA = OUT }| ).
        DATA(lv_kunwe_out)       = condense( |{ ls_ewa-kunwe       ALPHA = OUT }| ).
        DATA(lv_disposer_out)    = condense( |{ ls_ewa-disposer    ALPHA = OUT }| ).
        DATA(lv_transporter_out) = condense( |{ ls_ewa-transporter ALPHA = OUT }| ).
        DATA(lv_def_recycler_out)= condense( |{ lv_def_recycler    ALPHA = OUT }| ).
        DATA(lv_def_carrier_out) = condense( |{ lv_def_carrier     ALPHA = OUT }| ).

        " 6o Assemble payload
        DATA(ls_order) = VALUE ty_bms_order(
          status           = 'OK'
          order_number     = lv_order_number
          order            = lv_order_ref
          order_sheet      = condense( ls_ewa-watp_notenr )
          order_sheet_type = 'LS'

          customer = VALUE #(
            number        = lv_kunnr_out
            name1         = condense( ls_cust_bp-name1 )
            name2         = condense( ls_cust_bp-name2 )
            street        = condense( ls_cust_bp-street )
            street_number = condense( ls_cust_bp-house_num1 )
            zip_code      = condense( ls_cust_bp-post_code1 )
            city          = condense( ls_cust_bp-city1 ) )

          place_of_delivery = VALUE #(
            number        = lv_kunwe_out
            name1         = condense( ls_kunwe_bp-name1 )
            name2         = condense( ls_kunwe_bp-name2 )
            street        = condense( ls_kunwe_bp-street )
            street_number = condense( ls_kunwe_bp-house_num1 )
            zip_code      = condense( ls_kunwe_bp-post_code1 )
            city          = condense( ls_kunwe_bp-city1 ) )

          " Location is the physical container site — reuse Warenempfänger address
          location = VALUE #(
            number        = lv_kunwe_out
            name1         = condense( ls_kunwe_bp-name1 )
            street        = condense( ls_kunwe_bp-street )
            street_number = condense( ls_kunwe_bp-house_num1 )
            zip_code      = condense( ls_kunwe_bp-post_code1 )
            city          = condense( ls_kunwe_bp-city1 ) )

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

          " Producer = Auftraggeber (waste generator)
          producer = VALUE #(
            number        = lv_kunnr_out
            name1         = condense( ls_cust_bp-name1 )
            name2         = condense( ls_cust_bp-name2 )
            street        = condense( ls_cust_bp-street )
            street_number = condense( ls_cust_bp-house_num1 )
            zip_code      = condense( ls_cust_bp-post_code1 )
            city          = condense( ls_cust_bp-city1 ) )

          " Recycler: disposer from EWA or default from ZTOUR_BMS_CFG
          recycler = VALUE #(
            number        = COND #(
                              WHEN ls_ewa-disposer IS NOT INITIAL
                              THEN lv_disposer_out
                              ELSE lv_def_recycler_out )
            name1         = condense( ls_recycler_bp-name1 )
            name2         = condense( ls_recycler_bp-name2 )
            street        = condense( ls_recycler_bp-street )
            street_number = condense( ls_recycler_bp-house_num1 )
            zip_code      = condense( ls_recycler_bp-post_code1 )
            city          = condense( ls_recycler_bp-city1 ) )

          " Carrier: transporter from EWA or default from ZTOUR_BMS_CFG
          carrier = VALUE #(
            number        = COND #(
                              WHEN ls_ewa-transporter IS NOT INITIAL
                              THEN lv_transporter_out
                              ELSE lv_def_carrier_out )
            name1         = condense( ls_carrier_bp-name1 )
            name2         = condense( ls_carrier_bp-name2 )
            street        = condense( ls_carrier_bp-street )
            street_number = condense( ls_carrier_bp-house_num1 )
            zip_code      = condense( ls_carrier_bp-post_code1 )
            city          = condense( ls_carrier_bp-city1 ) )

          garbage_key  = condense( ls_ewa-watp_avvcode )
          garbage_name = condense( lv_material_desc )

          coll_consignment_note_nr =
            COND #( WHEN ls_ewa-watp_noteintnr IS NOT INITIAL
                    THEN condense( ls_ewa-watp_noteintnr ) )

          team               = condense( lv_team )
          contract_related   = abap_false
          signature_required = lv_sig_req
          positions          = lt_positions
          containers         = lt_containers ).

        "=====================================================================
        " STEP 7 — Serialize + patch field names
        "          /UI2/CL_JSON camelCase output does not match the BMS API
        "          which uses PascalCase throughout. Each field that appears
        "          in BMS validation errors gets an explicit REPLACE here.
        "          itemPrice:null is also patched to 0 for when positions
        "          are re-enabled after BMS confirms OptiAwiPositionType values.
        "=====================================================================
        DATA(lv_json) = /ui2/cl_json=>serialize(
          data        = ls_order
          pretty_name = /ui2/cl_json=>pretty_mode-camel_case ).

        " Structural rename — BMS field name differs from ABAP naming
        REPLACE ALL OCCURRENCES OF '"collConsignmentNoteNr"'
          IN lv_json WITH '"collectiveConsignmentNoteNumber"'.

        " Numeric null guard — decfloat16 zero serialises as null
        REPLACE ALL OCCURRENCES OF '"itemPrice":null'
          IN lv_json WITH '"itemPrice":0'.

        " PascalCase corrections — BMS API requires capital first letter
        REPLACE ALL OCCURRENCES OF '"orderNumber"'
          IN lv_json WITH '"OrderNumber"'.
        REPLACE ALL OCCURRENCES OF '"name1"'
          IN lv_json WITH '"Name1"'.
        REPLACE ALL OCCURRENCES OF '"name2"'
          IN lv_json WITH '"Name2"'.
        REPLACE ALL OCCURRENCES OF '"number"'
          IN lv_json WITH '"Number"'.
        REPLACE ALL OCCURRENCES OF '"street"'
          IN lv_json WITH '"Street"'.
        REPLACE ALL OCCURRENCES OF '"streetNumber"'
          IN lv_json WITH '"StreetNumber"'.
        REPLACE ALL OCCURRENCES OF '"zipCode"'
          IN lv_json WITH '"ZipCode"'.
        REPLACE ALL OCCURRENCES OF '"city"'
          IN lv_json WITH '"City"'.
        REPLACE ALL OCCURRENCES OF '"containerTypeName"'
          IN lv_json WITH '"ContainerTypeName"'.
        REPLACE ALL OCCURRENCES OF '"containerTypeNumber"'
          IN lv_json WITH '"ContainerTypeNumber"'.
        REPLACE ALL OCCURRENCES OF '"containerNumberOld"'
          IN lv_json WITH '"ContainerNumberOld"'.
        REPLACE ALL OCCURRENCES OF '"containerNumberNew"'
          IN lv_json WITH '"ContainerNumberNew"'.
        REPLACE ALL OCCURRENCES OF '"movementType"'
          IN lv_json WITH '"MovementType"'.
        REPLACE ALL OCCURRENCES OF '"customerOwned"'
          IN lv_json WITH '"CustomerOwned"'.
        REPLACE ALL OCCURRENCES OF '"quantity"'
          IN lv_json WITH '"Quantity"'.
        REPLACE ALL OCCURRENCES OF '"sortNumber"'
          IN lv_json WITH '"SortNumber"'.
        REPLACE ALL OCCURRENCES OF '"itemNumber"'
          IN lv_json WITH '"ItemNumber"'.
        REPLACE ALL OCCURRENCES OF '"itemDescription"'
          IN lv_json WITH '"ItemDescription"'.
        REPLACE ALL OCCURRENCES OF '"unit"'
          IN lv_json WITH '"Unit"'.
        REPLACE ALL OCCURRENCES OF '"positionType"'
          IN lv_json WITH '"PositionType"'.

        "=====================================================================
        " STEP 8 — POST to BMS
        "=====================================================================
        DATA lv_http_status TYPE i.
        DATA lv_response    TYPE string.
        CLEAR: lv_http_status, lv_response.

        zcl_wr_pd_tour_helper=>post_bms_order(
          EXPORTING
            iv_base_url     = lv_bms_base_url
            iv_bearer_token = lv_bearer_token
            iv_json         = lv_json
          IMPORTING
            ev_http_status  = lv_http_status
            ev_response     = lv_response ).

        zcl_wr_pd_tour_helper=>log_bms_call(
          iv_tour_uuid    = ls_tour-touruuid
          iv_service_uuid = ls_asgmt-serviceuuid
          iv_order_number = ls_ewa-smaufnr
          iv_endpoint     = '/api/container/create-order-halle'
          iv_http_status  = lv_http_status
          iv_request      = lv_json
          iv_response     = lv_response ).

        "=====================================================================
        " STEP 9 — Evaluate response
        "          BMS returns 200 OR 201 — both are success
        "=====================================================================
        IF lv_http_status = 200 OR lv_http_status = 201.
          APPEND VALUE #(
            %tky = ls_tour-%tky
            %msg = new_message(
                     id       = 'Z_MSG_SVR_TOUR_EXT'
                     number   = '010'
                     severity = if_abap_behv_message=>severity-success
                     v1       = lv_order_number )
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

        " Write per-service BMS status to /plce/tpdsrvcst
        DATA(lv_svc_bms_status) = COND string(
          WHEN lv_http_status = 200 OR lv_http_status = 201
          THEN 'FREIGEGEBEN'
          ELSE 'ERROR' ).

        UPDATE /plce/tpdsrvcst
          SET zz_bms_status = @lv_svc_bms_status
          WHERE service_uuid = @ls_asgmt-serviceuuid.

        IF sy-subrc <> 0.
          DATA ls_srvcst TYPE /plce/tpdsrvcst.
          CLEAR ls_srvcst.
          ls_srvcst-mandt         = sy-mandt.
          ls_srvcst-service_uuid  = ls_asgmt-serviceuuid.
          ls_srvcst-zz_bms_status = lv_svc_bms_status.
          INSERT /plce/tpdsrvcst FROM ls_srvcst.
        ENDIF.

      ENDLOOP. " lt_asgmts WHERE touruuid

      "=======================================================================
      " STEP 10 — Derive tour status from service statuses
      "=======================================================================
      DATA lv_count_ok    TYPE i VALUE 0.
      DATA lv_count_error TYPE i VALUE 0.

      LOOP AT lt_asgmts INTO DATA(ls_a_check)
        WHERE touruuid = ls_tour-touruuid.
        SELECT SINGLE zz_bms_status
          FROM /plce/tpdsrvcst
          WHERE service_uuid = @ls_a_check-serviceuuid
          INTO @DATA(lv_svc_stat).
        IF lv_svc_stat = 'FREIGEGEBEN'.
          lv_count_ok    = lv_count_ok + 1.
        ELSEIF lv_svc_stat = 'ERROR'.
          lv_count_error = lv_count_error + 1.
        ENDIF.
      ENDLOOP.

      DATA(lv_bms_status) = COND /plce/char20(
        WHEN lv_count_error = 0 AND lv_count_ok > 0  THEN 'SENT'
        WHEN lv_count_error > 0 AND lv_count_ok > 0  THEN 'PARTIAL'
        ELSE                                               'ERROR' ).

      MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
        ENTITY extcustom
          UPDATE FIELDS ( zz_bms_status )
          WITH VALUE #( ( touruuid = ls_tour-touruuid zz_bms_status = lv_bms_status ) )
        FAILED   DATA(lf_mod)
        REPORTED DATA(lr_mod).

      IF lf_mod IS NOT INITIAL.
        MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
          ENTITY tour
            CREATE BY \_extcustom
              FIELDS ( zz_bms_status )
              WITH VALUE #( ( %tky = ls_tour-%tky
                              %target = VALUE #( ( zz_bms_status = lv_bms_status ) ) ) )
          FAILED DATA(lf_crt) REPORTED DATA(lr_crt).
      ENDIF.

    ENDLOOP. " lt_tours

  ENDMETHOD.
