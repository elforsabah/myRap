METHOD stornoBMSService.

  "=======================================================================
  " CONFIG — read from ZTOUR_BMS_CFG (same as touranBMSfreigeben)
  "=======================================================================
SELECT SINGLE bms_endpoint_url,
              bms_username,
              bms_password,
              active
  FROM ztour_bms_cfg
  where  config_id = 'DEFAULT'
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

  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      FIELDS ( TourId TourUUID )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tours)
    FAILED DATA(lt_failed).

  CHECK lt_failed IS INITIAL.

  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour BY \_ServiceAssignments
      FIELDS ( TourUUID ServiceUUID Removed )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_asgmts).

  DELETE lt_asgmts WHERE Removed IS NOT INITIAL.

  DATA lt_svc_uuids TYPE RANGE OF /plce/pdservice_uuid.
  lt_svc_uuids = VALUE #(
    FOR ls IN lt_asgmts
    ( sign = 'I' option = 'EQ' low = ls-ServiceUUID ) ).

  TYPES: BEGIN OF ty_svc_storno,
           service_uuid     TYPE /plce/pdservice_uuid,
           reference_int_id TYPE char30,
         END OF ty_svc_storno.
  DATA lt_services TYPE HASHED TABLE OF ty_svc_storno
                   WITH UNIQUE KEY service_uuid.

  IF lt_svc_uuids IS NOT INITIAL.
    SELECT service_uuid,
           reference_int_id
      FROM /plce/tpdsrv
      WHERE service_uuid IN @lt_svc_uuids
      INTO CORRESPONDING FIELDS OF TABLE @lt_services.
  ENDIF.

  LOOP AT lt_tours INTO DATA(ls_tour).

    DATA lv_token TYPE string.
    DATA lv_error TYPE string.
    CLEAR: lv_token, lv_error.

    zcl_wr_pd_tour_helper=>get_bms_bearer_token(
      EXPORTING
        iv_base_url = lv_bms_base_url
        iv_username = lv_bms_user
        iv_password = lv_bms_password
      IMPORTING
        ev_token    = lv_token
        ev_error    = lv_error ).

    IF lv_error IS NOT INITIAL.
      APPEND VALUE #(
        %tky = ls_tour-%tky
        %msg = new_message_with_text(
                 severity = if_abap_behv_message=>severity-error
                 text     = lv_error )
      ) TO reported-tour.
      CONTINUE.
    ENDIF.

    LOOP AT lt_asgmts INTO DATA(ls_asgmt)
      WHERE TourUUID = ls_tour-TourUUID.

      DATA ls_svc TYPE ty_svc_storno.
      CLEAR ls_svc.
      READ TABLE lt_services INTO ls_svc
        WITH KEY service_uuid = ls_asgmt-ServiceUUID.
      CHECK sy-subrc = 0.

      SELECT SINGLE smaufnr
        FROM ewa_order_object
        WHERE pobjnr = @ls_svc-reference_int_id
        INTO @DATA(lv_smaufnr).
      CHECK sy-subrc = 0.

      DATA lv_http_status TYPE i.
      DATA lv_response    TYPE string.
      CLEAR: lv_http_status, lv_response.

      zcl_wr_pd_tour_helper=>storno_bms_order(
        EXPORTING
          iv_base_url     = lv_bms_base_url
          iv_bearer_token = lv_token
          iv_order_number = |{ lv_smaufnr ALPHA = OUT }|
          iv_full_json    = ''
        IMPORTING
          ev_http_status  = lv_http_status
          ev_response     = lv_response ).

      zcl_wr_pd_tour_helper=>log_bms_call(
        iv_tour_uuid    = ls_tour-TourUUID
        iv_service_uuid = ls_asgmt-ServiceUUID
        iv_order_number = lv_smaufnr
        iv_endpoint     = '/api/container/create-order-halle (STORNO)'
        iv_http_status  = lv_http_status
        iv_request      = |STORNIERT: { lv_smaufnr ALPHA = OUT }|
        iv_response     = lv_response ).

      IF lv_http_status = 200 OR lv_http_status = 201.

        UPDATE /plce/tpdsrvcst
          SET zz_bms_status = 'STORNIERT'
          WHERE service_uuid = @ls_asgmt-ServiceUUID.

        IF sy-subrc <> 0.
          DATA ls_srvcst TYPE /plce/tpdsrvcst.
          CLEAR ls_srvcst.
          ls_srvcst-service_uuid  = ls_asgmt-ServiceUUID.
          ls_srvcst-zz_bms_status = 'STORNIERT'.
          INSERT /plce/tpdsrvcst FROM ls_srvcst.
        ENDIF.

        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '015'
                   severity = if_abap_behv_message=>severity-success
                   v1       = |{ lv_smaufnr ALPHA = OUT }| )
        ) TO reported-tour.

      ELSE.

        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '016'
                   severity = if_abap_behv_message=>severity-error
                   v1       = |{ lv_smaufnr ALPHA = OUT }|
                   v2       = lv_response )
        ) TO reported-tour.

      ENDIF.

    ENDLOOP.

    MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY ExtCustom
        UPDATE FIELDS ( zz_bms_status )
        WITH VALUE #( ( TourUUID = ls_tour-TourUUID zz_bms_status = 'STORNIERT' ) )
      FAILED   DATA(lf_mod)
      REPORTED DATA(lr_mod).

    IF lf_mod IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          CREATE BY \_ExtCustom
            FIELDS ( zz_bms_status )
            WITH VALUE #( ( %tky = ls_tour-%tky
                            %target = VALUE #( ( zz_bms_status = 'STORNIERT' ) ) ) )
        FAILED DATA(lf_crt) REPORTED DATA(lr_crt).
    ENDIF.

  ENDLOOP.


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
      container_number_old  TYPE string,   " FIX: TIDNR at location
      container_number_new  TYPE string,   " FIX: TIDNR new container
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
      container_atloc_tidnr     TYPE char30,   " FIX: TIDNR at location → containerNumberOld
      container_new_tidnr       TYPE char30,   " FIX: TIDNR new → containerNumberNew
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

  DATA ls_empty_addr TYPE ty_address.

  "=======================================================================
  " CONFIG — read from ZTOUR_BMS_CFG (replaces hardcoded constants)
  "          FIX: credentials and URL are no longer hardcoded
  "=======================================================================
  SELECT SINGLE bms_endpoint_url,
                bms_username,
                bms_password,
                active
    FROM ztour_bms_cfg
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
  " STEP 2 — Read service assignments (link table: UUID + sequence)
  "=======================================================================
  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour BY \_ServiceAssignments
      FIELDS ( TourUUID ServiceUUID TourSequence Removed )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_asgmts).

  DELETE lt_asgmts WHERE Removed IS NOT INITIAL.
  SORT lt_asgmts BY TourUUID ASCENDING TourSequence ASCENDING.

  "=======================================================================
  " STEP 3 — Read service data from /plce/tpdsrv + /plce/tpdsrvwr
  "           FIX: LEFT OUTER JOIN to /plce/tpdsrvwr gives TIDNR fields
  "                for containerNumberOld and containerNumberNew
  "           READ ENTITIES of foreign root is forbidden — direct SELECT
  "=======================================================================
  DATA lt_svc_uuids TYPE RANGE OF /plce/pdservice_uuid.
  lt_svc_uuids = VALUE #(
    FOR ls IN lt_asgmts
    ( sign = 'I' option = 'EQ' low = ls-ServiceUUID ) ).

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
  "=======================================================================
  DATA lt_pobjnr TYPE RANGE OF j_objnr.
  lt_pobjnr = VALUE #(
    FOR lss IN lt_services
    WHERE ( reference_internal_id IS NOT INITIAL )
    ( sign = 'I' option = 'EQ' low = lss-reference_internal_id ) ).

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
  " STEP 6 — Main loop: one auth per tour, then POST per service
  "=======================================================================
  LOOP AT lt_tours INTO DATA(ls_tour).

    " 6a Kolonne lookup
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

      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY ExtCustom
          UPDATE FIELDS ( zz_bms_status )
          WITH VALUE #( ( TourUUID = ls_tour-TourUUID zz_bms_status = 'ERROR' ) )
        FAILED   DATA(lf_auth_mod)
        REPORTED DATA(lr_auth_mod).

      IF lf_auth_mod IS NOT INITIAL.
        MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
          ENTITY Tour
            CREATE BY \_ExtCustom
              FIELDS ( zz_bms_status )
              WITH VALUE #( ( %tky = ls_tour-%tky
                              %target = VALUE #( ( zz_bms_status = 'ERROR' ) ) ) )
          FAILED DATA(lf_auth_crt) REPORTED DATA(lr_auth_crt).
      ENDIF.
      CONTINUE.
    ENDIF.

    DATA lv_tour_has_error TYPE abap_bool VALUE abap_false.

    LOOP AT lt_asgmts INTO DATA(ls_asgmt)
      WHERE TourUUID = ls_tour-TourUUID.

      DATA ls_svc TYPE ty_service.
      CLEAR ls_svc.
      READ TABLE lt_services INTO ls_svc
        WITH KEY service_uuid = ls_asgmt-ServiceUUID.
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

      " 6c Movement type
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

      " 6d Container array
      "    FIX: container_number_old / container_number_new
      "         mapped from PROLOGA TIDNR fields via /plce/tpdsrvwr JOIN
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
      ENDIF.

      " 6e Positions from VBAP
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

      " 6f LEISTUNGSZEIT
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
      DATA ls_cust_bp     TYPE ty_bp_addr.
      DATA ls_kunwe_bp    TYPE ty_bp_addr.
      DATA ls_carrier_bp  TYPE ty_bp_addr.
      DATA ls_recycler_bp TYPE ty_bp_addr.
      CLEAR: ls_cust_bp, ls_kunwe_bp, ls_carrier_bp, ls_recycler_bp.

      READ TABLE lt_bp_addr INTO ls_cust_bp     WITH KEY partner = ls_ewa-kunnr.
      READ TABLE lt_bp_addr INTO ls_kunwe_bp    WITH KEY partner = ls_ewa-kunwe.
      READ TABLE lt_bp_addr INTO ls_carrier_bp  WITH KEY partner = ls_ewa-transporter.
      READ TABLE lt_bp_addr INTO ls_recycler_bp WITH KEY partner = ls_ewa-disposer.

      " 6j Material description (FIELD SWAP)
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

      " 6l Assemble payload
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

        location = VALUE #( street = ls_ewa-servloc )

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
      " STEP 7 — Serialize + patch long field name
      "=====================================================================
      DATA(lv_json) = /ui2/cl_json=>serialize(
        data        = ls_order
        pretty_name = /ui2/cl_json=>pretty_mode-camel_case ).

      REPLACE ALL OCCURRENCES OF '"collConsignmentNoteNr"'
        IN lv_json
        WITH '"collectiveConsignmentNoteNumber"'.

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
        iv_tour_uuid    = ls_tour-TourUUID
        iv_service_uuid = ls_asgmt-ServiceUUID
        iv_order_number = ls_ewa-smaufnr
        iv_endpoint     = '/api/container/create-order-halle'
        iv_http_status  = lv_http_status
        iv_request      = lv_json
        iv_response     = lv_response ).

      "=====================================================================
      " STEP 9 — Evaluate response
      "           FIX: BMS returns 200 OR 201 — both are success
      "=====================================================================
      IF lv_http_status = 200 OR lv_http_status = 201.
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

      " Write per-service BMS status to /plce/tpdsrvcst
      DATA(lv_svc_bms_status) = COND string(
        WHEN lv_http_status = 200 OR lv_http_status = 201
        THEN 'FREIGEGEBEN'
        ELSE 'ERROR' ).

      UPDATE /plce/tpdsrvcst
        SET zz_bms_status = @lv_svc_bms_status
        WHERE service_uuid = @ls_asgmt-ServiceUUID.

      IF sy-subrc <> 0.
        DATA ls_srvcst TYPE /plce/tpdsrvcst.
        CLEAR ls_srvcst.
        ls_srvcst-service_uuid  = ls_asgmt-ServiceUUID.
        ls_srvcst-zz_bms_status = lv_svc_bms_status.
        INSERT /plce/tpdsrvcst FROM ls_srvcst.
      ENDIF.

    ENDLOOP. " lt_asgmts WHERE TourUUID

    "=======================================================================
    " STEP 10 — Derive tour status from service statuses
    "=======================================================================
    DATA lv_count_ok    TYPE i VALUE 0.
    DATA lv_count_error TYPE i VALUE 0.

    LOOP AT lt_asgmts INTO DATA(ls_a_check)
      WHERE TourUUID = ls_tour-TourUUID.
      SELECT SINGLE zz_bms_status
        FROM /plce/tpdsrvcst
        WHERE service_uuid = @ls_a_check-ServiceUUID
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

    MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
      ENTITY ExtCustom
        UPDATE FIELDS ( zz_bms_status )
        WITH VALUE #( ( TourUUID = ls_tour-TourUUID zz_bms_status = lv_bms_status ) )
      FAILED   DATA(lf_mod)
      REPORTED DATA(lr_mod).

    IF lf_mod IS NOT INITIAL.
      MODIFY ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
        ENTITY Tour
          CREATE BY \_ExtCustom
            FIELDS ( zz_bms_status )
            WITH VALUE #( ( %tky = ls_tour-%tky
                            %target = VALUE #( ( zz_bms_status = lv_bms_status ) ) ) )
        FAILED DATA(lf_crt) REPORTED DATA(lr_crt).
    ENDIF.

  ENDLOOP. " lt_tours

ENDMETHOD.

ENDCLASS.

@EndUserText.label : 'BMS API Communication Log'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #A
@AbapCatalog.dataMaintenance : #RESTRICTED
define table zbms_api_log {

  key mandt       : mandt not null;
  key log_uuid    : sysuuid_x16 not null;
  created_at      : timestampl;
  created_by      : syuname;
  direction       : char10;
  tour_uuid       : /plce/pdtour_uuid;
  service_uuid    : /plce/pdservice_uuid;
  order_number    : aufnr;
  endpoint        : char255;
  http_status     : int4;
  request_payload : abap.char(1333);
  response_body   : abap.char(1333);

}


@EndUserText.label : 'BMS connection configuration'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #L
@AbapCatalog.dataMaintenance : #ALLOWED
define table ztour_bms_cfg {

  key mandt        : mandt not null;
  key config_id    : char10 not null;
  bms_endpoint_url : zbms_endpoint;
  bms_username     : zbms_endpoint;
  bms_password     : zbms_endpoint;
  active           : char1;
  description      : char80;

}


@EndUserText.label : 'Tour Template → BMS Kolonne Mapping'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #C
@AbapCatalog.dataMaintenance : #ALLOWED
define table ztour_bms_kolonn {

  key mandt         : mandt not null;
  key tour_template : /plce/pdtour_template not null;
  bms_team          : text40;

}


@EndUserText.label : 'Bermekungen für Storno'
@AbapCatalog.enhancement.category : #NOT_EXTENSIBLE
@AbapCatalog.tableCategory : #TRANSPARENT
@AbapCatalog.deliveryClass : #C
@AbapCatalog.dataMaintenance : #ALLOWED
define table zwr_pd_storno_bm {

  key mandt    : mandt not null;
  key conftype : conftype not null;
  bemerkung    : zz_bemerkung;

}



  class ZCL_WR_PD_TOUR_HELPER definition
  public
  create public .

public section.

  class-methods GET_BMS_BEARER_TOKEN
    importing
      !IV_BASE_URL type CHAR60
      !IV_USERNAME type CHAR60
      !IV_PASSWORD type CHAR60
    exporting
      !EV_TOKEN type STRING
      !EV_ERROR type STRING .
  class-methods POST_BMS_ORDER
    importing
      !IV_BASE_URL type CHAR60
      !IV_BEARER_TOKEN type STRING
      !IV_JSON type STRING
    exporting
      !EV_HTTP_STATUS type I
      !EV_RESPONSE type STRING .
  class-methods STORNO_BMS_ORDER
    importing
      !IV_BASE_URL type CHAR60
      !IV_BEARER_TOKEN type STRING
      !IV_ORDER_NUMBER type STRING
      !IV_FULL_JSON type STRING
    exporting
      !EV_HTTP_STATUS type I
      !EV_RESPONSE type STRING .
  class-methods LOG_BMS_CALL
    importing
      !IV_TOUR_UUID type /PLCE/PDTOUR_UUID
      !IV_SERVICE_UUID type /PLCE/PDSERVICE_UUID
      !IV_ORDER_NUMBER type AUFNR
      !IV_ENDPOINT type STRING
      !IV_HTTP_STATUS type I
      !IV_REQUEST type STRING
      !IV_RESPONSE type STRING .
ENDCLASS.



CLASS ZCL_WR_PD_TOUR_HELPER IMPLEMENTATION.


  METHOD get_bms_bearer_token.

    CLEAR: ev_token, ev_error.

    DATA(lv_auth_json) = |\{ "username": "{ iv_username }", | &&
                          |"password": "{ iv_password }" \}|.

    DATA lo_http TYPE REF TO if_http_client.

    cl_http_client=>create_by_url(
      EXPORTING url    = iv_base_url && '/api/user/authenticate/user'
      IMPORTING client = lo_http
      EXCEPTIONS OTHERS = 4 ).

    IF sy-subrc <> 0.
      ev_error = 'BMS: auth HTTP client creation failed'.
      RETURN.
    ENDIF.

    lo_http->request->set_method( if_http_request=>co_request_method_post ).
    lo_http->request->set_header_field(
      name  = 'Content-Type'
      value = 'application/json' ).
    lo_http->request->set_cdata( lv_auth_json ).

    lo_http->send(    EXCEPTIONS OTHERS = 4 ).
    lo_http->receive( EXCEPTIONS OTHERS = 4 ).

    DATA lv_code TYPE i.
    lo_http->response->get_status( IMPORTING code = lv_code ).
    DATA(lv_body) = lo_http->response->get_cdata( ).
    lo_http->close( ).

    IF lv_code <> 200.
      ev_error = |BMS auth failed — HTTP { lv_code }: { lv_body }|.
      RETURN.
    ENDIF.

    TYPES: BEGIN OF ty_auth_data,
             access_token TYPE string,
           END OF ty_auth_data,
           BEGIN OF ty_auth_response,
             data TYPE ty_auth_data,
           END OF ty_auth_response.
    DATA ls_auth TYPE ty_auth_response.

    /ui2/cl_json=>deserialize(
      EXPORTING
        json        = lv_body
        pretty_name = /ui2/cl_json=>pretty_mode-camel_case
      CHANGING
        data        = ls_auth ).

    IF ls_auth-data-access_token IS INITIAL.
      ev_error = |BMS auth: no accessToken in response. Body: { lv_body }|.
      RETURN.
    ENDIF.

    ev_token = |Bearer { ls_auth-data-access_token }|.

  ENDMETHOD.


  METHOD post_bms_order.

    CLEAR: ev_http_status, ev_response.

    DATA lo_http TYPE REF TO if_http_client.

    cl_http_client=>create_by_url(
      EXPORTING url    = iv_base_url && '/api/container/create-order-halle'
      IMPORTING client = lo_http
      EXCEPTIONS OTHERS = 4 ).

    IF sy-subrc <> 0.
      ev_http_status = 0.
      ev_response    = 'BMS: order HTTP client creation failed'.
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

    DATA lv_code TYPE i.
    lo_http->response->get_status( IMPORTING code = lv_code ).
    ev_http_status = lv_code.
    ev_response    = lo_http->response->get_cdata( ).

    lo_http->close( ).

  ENDMETHOD.


  METHOD storno_bms_order.

    CLEAR: ev_http_status, ev_response.

    " Build JSON — minimal storno if no full payload provided,
    " otherwise replace status field in the existing JSON
    DATA(lv_json) = COND string(
      WHEN iv_full_json IS INITIAL
      THEN |\{ "status": "STORNIERT", "orderNumber": "{ iv_order_number }" \}|
      ELSE iv_full_json ).

    IF iv_full_json IS NOT INITIAL.
      REPLACE FIRST OCCURRENCE OF '"status":"OK"'
        IN lv_json WITH '"status":"STORNIERT"'.
    ENDIF.

    DATA lo_http TYPE REF TO if_http_client.

    cl_http_client=>create_by_url(
      EXPORTING url    = iv_base_url && '/api/container/create-order-halle'
      IMPORTING client = lo_http
      EXCEPTIONS OTHERS = 4 ).

    IF sy-subrc <> 0.
      ev_http_status = 0.
      ev_response    = 'BMS Storno: HTTP client creation failed'.
      RETURN.
    ENDIF.

    lo_http->request->set_method( if_http_request=>co_request_method_post ).
    lo_http->request->set_header_field(
      name  = 'Content-Type'
      value = 'application/json-patch+json' ).
    lo_http->request->set_header_field(
      name  = 'Authorization'
      value = iv_bearer_token ).
    lo_http->request->set_cdata( lv_json ).

    lo_http->send(    EXCEPTIONS OTHERS = 4 ).
    lo_http->receive( EXCEPTIONS OTHERS = 4 ).

    DATA lv_code TYPE i.
    lo_http->response->get_status( IMPORTING code = lv_code ).
    ev_http_status = lv_code.
    ev_response    = lo_http->response->get_cdata( ).

    lo_http->close( ).

  ENDMETHOD.


 METHOD log_bms_call.

    DATA ls_log TYPE zbms_api_log.
    ls_log-log_uuid        = cl_system_uuid=>create_uuid_x16_static( ).
    GET TIME STAMP FIELD ls_log-created_at.
    ls_log-created_by      = sy-uname.
    ls_log-direction       = 'OUTBOUND'.
    ls_log-tour_uuid       = iv_tour_uuid.
    ls_log-service_uuid    = iv_service_uuid.
    ls_log-order_number    = iv_order_number.
    ls_log-endpoint        = iv_endpoint.
    ls_log-http_status     = iv_http_status.
    ls_log-request_payload = iv_request.
    ls_log-response_body   = iv_response.
    INSERT zbms_api_log FROM ls_log.

  ENDMETHOD.
ENDCLASS.



ENDMETHOD.
