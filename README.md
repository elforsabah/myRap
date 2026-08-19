METHOD touranbmsfreigeben.

*----------------------------------------------------------------------*
* TYPES
*----------------------------------------------------------------------*
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
      " named so camelCase serialisation already produces the BMS field
      " name — replaces the REPLACE that used to patch this
      collective_consignment_note_number TYPE string,
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
      beh_type       TYPE beh_type,          " CHAR 40, "Behältertyp"
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
      sdaufnr        TYPE vbeln_va,
      sdposnr        TYPE posnr,
    END OF ty_ewaobj,
    " POBJNR is not a key field of EWA_ORDER_OBJECT (key is ORDERNR +
    " ORDER_LAUFNR). Verify it is unique, otherwise this dumps.
    tt_ewaobj TYPE HASHED TABLE OF ty_ewaobj WITH UNIQUE KEY pobjnr,

    " container fields removed — they are 1:n and live in tt_srvwr
    BEGIN OF ty_service,
      service_uuid              TYPE /plce/pdservice_uuid,
      service_id                TYPE /plce/pdservice_id,
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
    tt_services TYPE HASHED TABLE OF ty_service WITH UNIQUE KEY service_uuid,

    BEGIN OF ty_srvwr,
      service_uuid          TYPE /plce/pdservice_uuid,
      container_atloc_tidnr TYPE char30,
      container_new_tidnr   TYPE char30,
    END OF ty_srvwr,
    tt_srvwr TYPE SORTED TABLE OF ty_srvwr WITH NON-UNIQUE KEY service_uuid,

    " NON-UNIQUE: a business partner legitimately has several addresses
    BEGIN OF ty_bp_addr,
      partner    TYPE bu_partner,
      addrnumber TYPE ad_addrnum,
      name1      TYPE ad_name1,
      name2      TYPE ad_name2,
      street     TYPE ad_street,
      house_num1 TYPE ad_hsnm1,
      post_code1 TYPE ad_pstcd1,
      city1      TYPE ad_city1,
    END OF ty_bp_addr,
    tt_bp_addr TYPE SORTED TABLE OF ty_bp_addr WITH NON-UNIQUE KEY partner,

    BEGIN OF ty_bp_key,
      partner TYPE bu_partner,
    END OF ty_bp_key,
    tt_bp_key TYPE SORTED TABLE OF ty_bp_key WITH UNIQUE KEY partner.

*----------------------------------------------------------------------*
* DATA — declared once, cleared where it belongs
*----------------------------------------------------------------------*
  DATA:
    lt_services      TYPE tt_services,
    lt_srvwr         TYPE tt_srvwr,
    lt_ewaobj        TYPE tt_ewaobj,
    lt_bp_keys       TYPE tt_bp_key,
    lt_bp_addr       TYPE tt_bp_addr,
    lt_containers    TYPE tt_containers,
    lt_positions     TYPE tt_positions,
    ls_svc           TYPE ty_service,
    ls_ewa           TYPE ty_ewaobj,
    ls_cust_bp       TYPE ty_bp_addr,
    ls_kunwe_bp      TYPE ty_bp_addr,
    ls_carrier_bp    TYPE ty_bp_addr,
    ls_recycler_bp   TYPE ty_bp_addr,
    ls_srvcst        TYPE /plce/tpdsrvcst,
    lv_bearer_token  TYPE string,
    lv_auth_error    TYPE string,
    lv_movement_type TYPE string,
    lv_ctype_number  TYPE string,
    lv_ctype_name    TYPE string,
    lv_material_desc TYPE maktx,
    lv_http_status   TYPE i,
    lv_response      TYPE string,
    lv_json          TYPE string,
    lv_upd_subrc     TYPE sy-subrc,
    lv_count_ok      TYPE i,
    lv_count_error   TYPE i.

  " for parsing the documented BMS error body
  DATA: BEGIN OF ls_err_body,
          BEGIN OF error,
            message   TYPE string,
            timestamp TYPE string,
          END OF error,
        END OF ls_err_body.

*----------------------------------------------------------------------*
* CONFIG
*----------------------------------------------------------------------*
  SELECT SINGLE bms_endpoint_url, bms_username, bms_password,
                active, default_carrier, default_recycler
    FROM ztour_bms_cfg
    WHERE config_id = 'DEFAULT'
    INTO @DATA(ls_cfg).

  IF sy-subrc <> 0 OR ls_cfg-active <> abap_true.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ky>).
      APPEND VALUE #(
        %tky = <ky>-%tky
        %msg = new_message(
                 id       = 'Z_MSG_SVR_TOUR_EXT'
                 number   = '018'          " BMS-Schnittstelle nicht aktiv
                 severity = if_abap_behv_message=>severity-error )
      ) TO reported-tour.
    ENDLOOP.
    RETURN.
  ENDIF.

  DATA(lv_bms_base_url) = ls_cfg-bms_endpoint_url.
  DATA(lv_bms_user)     = ls_cfg-bms_username.
  DATA(lv_bms_password) = ls_cfg-bms_password.
  DATA(lv_def_carrier)  = ls_cfg-default_carrier.
  DATA(lv_def_recycler) = ls_cfg-default_recycler.

*----------------------------------------------------------------------*
* STEP 1 — tours
*----------------------------------------------------------------------*
  READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
    ENTITY tour
      FIELDS ( tourid tourtemplate tourstatus startdate )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tours)
    FAILED DATA(lt_failed).

  " report unreadable tours and carry on — the old CHECK exited the
  " method and silently dropped every selected tour
  LOOP AT lt_failed-tour ASSIGNING FIELD-SYMBOL(<fail>).
    APPEND VALUE #(
      %tky = <fail>-%tky
      %msg = new_message(
               id       = 'Z_MSG_SVR_TOUR_EXT'
               number   = '024'
               severity = if_abap_behv_message=>severity-error )
    ) TO reported-tour.
  ENDLOOP.

  IF lt_tours IS INITIAL.
    RETURN.
  ENDIF.

*----------------------------------------------------------------------*
* STEP 2 — service assignments
*----------------------------------------------------------------------*
  READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
    ENTITY tour BY \_serviceassignments
      FIELDS ( touruuid serviceuuid toursequence removed )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_asgmts).

  DELETE lt_asgmts WHERE removed IS NOT INITIAL.
  SORT lt_asgmts BY touruuid ASCENDING toursequence ASCENDING.

  IF lt_asgmts IS INITIAL.
    RETURN.
  ENDIF.

*----------------------------------------------------------------------*
* STEP 3 — services and containers, read separately
*          The old LEFT OUTER JOIN could return several rows per
*          SERVICE_UUID and dumped on the HASHED unique key.
*----------------------------------------------------------------------*
  SELECT s~service_uuid,
         s~service_id,
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
         s~functional_location
    FROM /plce/tpdsrv AS s
    FOR ALL ENTRIES IN @lt_asgmts
    WHERE s~service_uuid = @lt_asgmts-serviceuuid
    INTO CORRESPONDING FIELDS OF TABLE @lt_services.

  SELECT w~service_uuid,
         w~container_atloc_tidnr,
         w~container_new_tidnr
    FROM /plce/tpdsrvwr AS w
    FOR ALL ENTRIES IN @lt_asgmts
    WHERE w~service_uuid = @lt_asgmts-serviceuuid
    INTO CORRESPONDING FIELDS OF TABLE @lt_srvwr.

*----------------------------------------------------------------------*
* STEP 4 — EWA order objects
*----------------------------------------------------------------------*
  IF lt_services IS NOT INITIAL.
    SELECT pobjnr, ordernr, smaufnr, ordertxt, servloc,
           kunnr, kunwe, beh_type, beh_anzahl, order_type,
           transporter, disposer, intappno, appno,
           planned_time, planned_durt, old_order_date, zz_order_date,
           /watp/avvcode   AS watp_avvcode,
           /watp/notenr    AS watp_notenr,
           /watp/noteintnr AS watp_noteintnr,
           sdaufnr, sdposnr
      FROM ewa_order_object
      FOR ALL ENTRIES IN @lt_services
      WHERE pobjnr = @lt_services-reference_internal_id
      INTO CORRESPONDING FIELDS OF TABLE @lt_ewaobj.
  ENDIF.

*----------------------------------------------------------------------*
* STEP 5 — business partner addresses
*          Unique sorted key table deduplicates for free. The old
*          DELETE ADJACENT DUPLICATES ran on an unsorted table.
*----------------------------------------------------------------------*
  LOOP AT lt_ewaobj ASSIGNING FIELD-SYMBOL(<e>).
    INSERT VALUE #( partner = <e>-kunnr )       INTO TABLE lt_bp_keys.
    INSERT VALUE #( partner = <e>-kunwe )       INTO TABLE lt_bp_keys.
    INSERT VALUE #( partner = <e>-transporter ) INTO TABLE lt_bp_keys.
    INSERT VALUE #( partner = <e>-disposer )    INTO TABLE lt_bp_keys.
  ENDLOOP.

  INSERT VALUE #( partner = lv_def_carrier )  INTO TABLE lt_bp_keys.
  INSERT VALUE #( partner = lv_def_recycler ) INTO TABLE lt_bp_keys.
  DELETE lt_bp_keys WHERE partner IS INITIAL.

  IF lt_bp_keys IS NOT INITIAL.
    SELECT ba~partner, ba~addrnumber,
           adr~name1, adr~name2, adr~street,
           adr~house_num1, adr~post_code1, adr~city1
      FROM but020 AS ba
      INNER JOIN adrc AS adr ON  adr~addrnumber = ba~addrnumber
                             AND adr~nation     = @space
      FOR ALL ENTRIES IN @lt_bp_keys
      WHERE ba~partner    = @lt_bp_keys-partner
        AND ba~date_from <= @sy-datum
        AND ba~date_to   >= @sy-datum
*       AND ba~xdfadr     = @abap_true   "<- enable after checking SE11
      INTO CORRESPONDING FIELDS OF TABLE @lt_bp_addr.

    SORT lt_bp_addr BY partner ASCENDING addrnumber ASCENDING.
  ENDIF.

*----------------------------------------------------------------------*
* STEP 6 — main loop
*----------------------------------------------------------------------*
  LOOP AT lt_tours INTO DATA(ls_tour).

    " per-tour reset. The old code used DATA ... VALUE 0 inside the loop,
    " which initialises once at method entry — the counters accumulated
    " across tours and every tour after the first error came out PARTIAL.
    CLEAR: lv_count_ok, lv_count_error.

    DATA(lv_tour_id_out) = condense( |{ ls_tour-tourid ALPHA = OUT }| ).

    " --- 6a Kolonne -----------------------------------------------------
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
                 severity = if_abap_behv_message=>severity-warning
                 v1       = lv_tour_id_out
                 v2       = ls_tour-tourtemplate )
      ) TO reported-tour.
      CLEAR lv_team.
    ENDIF.

    " --- 6b Authentication ----------------------------------------------
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
          WITH VALUE #( ( touruuid      = ls_tour-touruuid
                          zz_bms_status = 'ERROR' ) )
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
          FAILED   DATA(lf_auth_crt)
          REPORTED DATA(lr_auth_crt).

        " the old code discarded these — a failed status write was invisible
        IF lf_auth_crt IS NOT INITIAL.
          APPEND LINES OF lr_auth_crt-tour TO reported-tour.
        ENDIF.
      ENDIF.

      CONTINUE.
    ENDIF.

    " --- service loop ---------------------------------------------------
    LOOP AT lt_asgmts INTO DATA(ls_asgmt)
      WHERE touruuid = ls_tour-touruuid.

      CLEAR: ls_svc, ls_ewa, lt_containers, lt_positions,
             ls_cust_bp, ls_kunwe_bp, ls_carrier_bp, ls_recycler_bp,
             lv_movement_type, lv_ctype_number, lv_ctype_name,
             lv_material_desc, lv_http_status, lv_response, lv_json.

      " was CHECK sy-subrc = 0 — skipped the service with no message and
      " without counting it, so the tour could still be reported as SENT
      READ TABLE lt_services INTO ls_svc
        WITH TABLE KEY service_uuid = ls_asgmt-serviceuuid.

      IF sy-subrc <> 0.
        lv_count_error = lv_count_error + 1.
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

      DATA(lv_svc_id_out)  = condense( |{ ls_svc-service_id ALPHA = OUT }| ).
      DATA(lv_svc_ref_out) = condense( |{ ls_svc-reference_id ALPHA = OUT }| ).

      READ TABLE lt_ewaobj INTO ls_ewa
        WITH TABLE KEY pobjnr = ls_svc-reference_internal_id.

      IF sy-subrc <> 0.
        lv_count_error = lv_count_error + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '012'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_svc_ref_out )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      " --- 6c Movement type ----------------------------------------------
      " No fallback any more. Sending 'S' for an unmapped Umleerung would
      " create a WRONG order on the driver's tablet — worse than none.
      SELECT SINGLE bms_movement_type
        FROM zwrtwaalaprctp
        WHERE service_type = @ls_svc-action
        INTO @lv_movement_type.

      IF sy-subrc <> 0 OR lv_movement_type IS INITIAL.
        lv_count_error = lv_count_error + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '026'
                   severity = if_abap_behv_message=>severity-error
                   v1       = ls_svc-action
                   v2       = lv_svc_ref_out
                   v3       = lv_tour_id_out )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      " --- 6c2 Container type ----------------------------------------------
      " BEH_TYPE is passed straight through — no mapping table. CHAR 40
      " carries trailing blanks, so condense is essential: BMS will not
      " match "AC 10                     " against its catalogue.
      IF ls_ewa-beh_type IS INITIAL.
        lv_count_error = lv_count_error + 1.
        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '023'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_svc_ref_out )
        ) TO reported-tour.
        CONTINUE.
      ENDIF.

      lv_ctype_number = condense( ls_ewa-beh_type ).
      lv_ctype_name   = lv_ctype_number.

      DATA(lv_is_express) = xsdbool( ls_ewa-order_type = '02' ).

      " --- 6d Containers ---------------------------------------------------
      " One entry per row in /plce/tpdsrvwr (a service can carry several
      " containers). If the service has no container rows at all, fall back
      " to a single entry carrying BEH_ANZAHL.
      LOOP AT lt_srvwr ASSIGNING FIELD-SYMBOL(<wr>)
        WHERE service_uuid = ls_svc-service_uuid.
        APPEND VALUE #(
          quantity              = 1
          movement_type         = lv_movement_type
          container_type_number = lv_ctype_number
          container_type_name   = lv_ctype_name
          container_number_old  = condense( <wr>-container_atloc_tidnr )
          container_number_new  = condense( <wr>-container_new_tidnr )
          customer_owned        = abap_false
        ) TO lt_containers.
      ENDLOOP.

      IF lt_containers IS INITIAL.
        APPEND VALUE #(
          quantity              = COND i( WHEN ls_ewa-beh_anzahl > 0
                                          THEN ls_ewa-beh_anzahl ELSE 1 )
          movement_type         = lv_movement_type
          container_type_number = lv_ctype_number
          container_type_name   = lv_ctype_name
          customer_owned        = abap_false
        ) TO lt_containers.
      ENDIF.

      " --- 6e Positions ----------------------------------------------------
      " Still empty: OptiAwiPositionType enum not confirmed by BMS.
      " The Swagger sample shows "P" is accepted; the full catalogue is
      " needed before deriving the type from the material prefix.

      " --- 6f Service window, fixed format ---------------------------------
      " TIME = USER made the payload depend on the dispatcher's personal
      " settings — two users sent different JSON for the same tour.
      DATA(lv_win_start) = COND string(
        WHEN ls_svc-service_window_start_time IS NOT INITIAL
        THEN |{ ls_svc-service_window_start_time+0(2) }:| &&
             |{ ls_svc-service_window_start_time+2(2) }| ).

      DATA(lv_win_end) = COND string(
        WHEN ls_svc-service_window_end_time IS NOT INITIAL
        THEN |{ ls_svc-service_window_end_time+0(2) }:| &&
             |{ ls_svc-service_window_end_time+2(2) }| ).

      DATA(lv_plan_time) = COND string(
        WHEN ls_ewa-planned_time IS NOT INITIAL
        THEN |{ ls_ewa-planned_time+0(2) }:{ ls_ewa-planned_time+2(2) }| ).

      DATA(lv_exec_time) = COND string(
        WHEN lv_win_start IS NOT INITIAL AND lv_win_end IS NOT INITIAL
         AND lv_win_start <> lv_win_end    THEN |{ lv_win_start } bis { lv_win_end }|
        WHEN lv_win_start IS NOT INITIAL
         AND lv_win_end   IS NOT INITIAL   THEN lv_win_start
        WHEN lv_win_start IS NOT INITIAL   THEN |ab { lv_win_start }|
        WHEN lv_win_end   IS NOT INITIAL   THEN |bis { lv_win_end }|
        WHEN lv_plan_time IS NOT INITIAL   THEN lv_plan_time
        ELSE CONV string( ls_svc-service_window ) ).

      " --- 6g Signature ----------------------------------------------------
      " was 'UTERSCHR' — missing N, so this never matched and
      " signature_required was permanently false
      DATA(lv_sig_req) = xsdbool(
        to_upper( ls_ewa-ordertxt )        CS 'UNTERSCHR' OR
        to_upper( ls_svc-additional_text ) CS 'UNTERSCHR' ).

      " --- 6h Notes ---------------------------------------------------------
      DATA(lv_notes) = CONV string( ls_ewa-ordertxt ).

      " TODO: the KVV rule below is hardcoded and contradicts the
      " "everything in maintenance tables" principle. Two remarks:
      "   - KUNWE is numeric, so KUNWE CS 'IS' / 'RKT' can never be true
      "   - CS 'IS' on SMAUFNR is very broad and will produce false hits
      " Left unchanged because it is a business rule, not a defect.
      IF ls_ewa-smaufnr CS 'IS'  OR ls_ewa-kunwe CS 'IS'
      OR ls_ewa-smaufnr CS 'RKT' OR ls_ewa-kunwe CS 'RKT'
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

      " --- 6i Business partners --------------------------------------------
      " Decide the partner FIRST, then read number and address from that one
      " decision. The old code could send the transporter's number together
      " with the default carrier's address.
      DATA(lv_carrier_bp_no) = COND bu_partner(
        WHEN ls_ewa-transporter IS NOT INITIAL THEN ls_ewa-transporter
        ELSE lv_def_carrier ).

      DATA(lv_recycler_bp_no) = COND bu_partner(
        WHEN ls_ewa-disposer IS NOT INITIAL THEN ls_ewa-disposer
        ELSE lv_def_recycler ).

      READ TABLE lt_bp_addr INTO ls_cust_bp
        WITH KEY partner = ls_ewa-kunnr BINARY SEARCH.
      READ TABLE lt_bp_addr INTO ls_kunwe_bp
        WITH KEY partner = ls_ewa-kunwe BINARY SEARCH.
      READ TABLE lt_bp_addr INTO ls_carrier_bp
        WITH KEY partner = lv_carrier_bp_no BINARY SEARCH.
      READ TABLE lt_bp_addr INTO ls_recycler_bp
        WITH KEY partner = lv_recycler_bp_no BINARY SEARCH.

      " --- 6j Waste description ---------------------------------------------
      IF ls_ewa-watp_avvcode IS NOT INITIAL.
        SELECT SINGLE maktx
          FROM makt
          WHERE matnr = @ls_ewa-watp_avvcode
            AND spras = @sy-langu
          INTO @lv_material_desc.
      ENDIF.

      " --- 6k Duration HHMMSS -> minutes ------------------------------------
      DATA(lv_duration_min) = COND i(
        WHEN ls_ewa-planned_durt CO ' 0123456789'
        THEN ls_ewa-planned_durt(2) * 60 + ls_ewa-planned_durt+2(2)
        ELSE 0 ).

      " --- 6l/6m Order references -------------------------------------------
      DATA(lv_ordernr_clean) = condense( |{ ls_ewa-ordernr ALPHA = OUT }| ).
      DATA(lv_sdaufnr_clean) = condense( |{ ls_ewa-sdaufnr ALPHA = OUT }| ).
      DATA(lv_smaufnr_clean) = condense( |{ ls_ewa-smaufnr ALPHA = OUT }| ).

      DATA(lv_order_ref) = COND string(
        WHEN lv_ordernr_clean IS NOT INITIAL THEN lv_ordernr_clean
        WHEN lv_sdaufnr_clean IS NOT INITIAL THEN lv_sdaufnr_clean
        WHEN lv_smaufnr_clean IS NOT INITIAL THEN lv_smaufnr_clean
        ELSE                                      lv_svc_ref_out ).

      DATA(lv_order_number) = COND string(
        WHEN lv_smaufnr_clean IS NOT INITIAL THEN lv_smaufnr_clean
        ELSE lv_order_ref ).

      " --- 6n Trimmed partner numbers ----------------------------------------
      DATA(lv_kunnr_out)    = condense( |{ ls_ewa-kunnr ALPHA = OUT }| ).
      DATA(lv_kunwe_out)    = condense( |{ ls_ewa-kunwe ALPHA = OUT }| ).
      DATA(lv_carrier_out)  = condense( |{ lv_carrier_bp_no  ALPHA = OUT }| ).
      DATA(lv_recycler_out) = condense( |{ lv_recycler_bp_no ALPHA = OUT }| ).

      " --- 6o Dates ----------------------------------------------------------
      " The old ELSE branch had no guard, so an empty date produced
      " "0000-00-00T00:00:00.000Z".
      DATA(lv_planned_date) = COND string(
        WHEN ls_svc-earliest_date IS NOT INITIAL
        THEN |{ ls_svc-earliest_date DATE = ISO }T00:00:00.000Z|
        WHEN ls_ewa-zz_order_date IS NOT INITIAL
        THEN |{ ls_ewa-zz_order_date DATE = ISO }T00:00:00.000Z| ).

      DATA(lv_execution_date) = COND string(
        WHEN ls_svc-requested_date IS NOT INITIAL
        THEN |{ ls_svc-requested_date DATE = ISO }T00:00:00.000Z|
        WHEN ls_ewa-old_order_date IS NOT INITIAL
        THEN |{ ls_ewa-old_order_date DATE = ISO }T00:00:00.000Z| ).

      " --- 6p Assemble payload ------------------------------------------------
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

        " physical container site = Warenempfänger
        location = VALUE #(
          number        = lv_kunwe_out
          name1         = condense( ls_kunwe_bp-name1 )
          street        = condense( ls_kunwe_bp-street )
          street_number = condense( ls_kunwe_bp-house_num1 )
          zip_code      = condense( ls_kunwe_bp-post_code1 )
          city          = condense( ls_kunwe_bp-city1 ) )

        estimated_duration         = lv_duration_min
        planned_date               = lv_planned_date
        execution_date             = lv_execution_date
        execution_time_frame_start = lv_win_start
        execution_time_frame_end   = lv_win_end
        execution_time             = lv_exec_time
        notes                      = lv_notes

        special_notes = COND string(
          WHEN lv_is_express = abap_true
          THEN |EXPRESSAUFTRAG| &&
               COND string( WHEN ls_svc-additional_text IS NOT INITIAL
                            THEN | - | && ls_svc-additional_text )
          ELSE ls_svc-additional_text )

        " Auftraggeber is also the waste generator
        producer = VALUE #(
          number        = lv_kunnr_out
          name1         = condense( ls_cust_bp-name1 )
          name2         = condense( ls_cust_bp-name2 )
          street        = condense( ls_cust_bp-street )
          street_number = condense( ls_cust_bp-house_num1 )
          zip_code      = condense( ls_cust_bp-post_code1 )
          city          = condense( ls_cust_bp-city1 ) )

        recycler = VALUE #(
          number        = lv_recycler_out
          name1         = condense( ls_recycler_bp-name1 )
          name2         = condense( ls_recycler_bp-name2 )
          street        = condense( ls_recycler_bp-street )
          street_number = condense( ls_recycler_bp-house_num1 )
          zip_code      = condense( ls_recycler_bp-post_code1 )
          city          = condense( ls_recycler_bp-city1 ) )

        carrier = VALUE #(
          number        = lv_carrier_out
          name1         = condense( ls_carrier_bp-name1 )
          name2         = condense( ls_carrier_bp-name2 )
          street        = condense( ls_carrier_bp-street )
          street_number = condense( ls_carrier_bp-house_num1 )
          zip_code      = condense( ls_carrier_bp-post_code1 )
          city          = condense( ls_carrier_bp-city1 ) )

        garbage_key  = condense( ls_ewa-watp_avvcode )
        garbage_name = condense( lv_material_desc )

        collective_consignment_note_number =
          condense( ls_ewa-watp_noteintnr )

        team               = condense( lv_team )
        contract_related   = abap_false
        signature_required = lv_sig_req
        positions          = lt_positions
        containers         = lt_containers ).

*----------------------------------------------------------------------*
* STEP 7 — Serialize
*          The 20 blind REPLACE statements are gone. The BMS API binds
*          JSON case-insensitively (its own Swagger sample is camelCase),
*          and the only genuine name difference —
*          collConsignmentNoteNr -> collectiveConsignmentNoteNumber — is
*          now solved by the ABAP component name.
*          compress = abap_true omits initial fields, matching the shape
*          of the sample BMS accepted, and removes "itemPrice":null at
*          the source.
*----------------------------------------------------------------------*
      lv_json = /ui2/cl_json=>serialize(
        data        = ls_order
        compress    = abap_true
        pretty_name = /ui2/cl_json=>pretty_mode-camel_case ).

*----------------------------------------------------------------------*
* STEP 8 — POST
*----------------------------------------------------------------------*
      zcl_wr_pd_tour_helper=>post_bms_order(
        EXPORTING
          iv_base_url     = lv_bms_base_url
          iv_bearer_token = lv_bearer_token
          iv_json         = lv_json
        IMPORTING
          ev_http_status  = lv_http_status
          ev_response     = lv_response ).

      " log the order number that was actually SENT — the old code logged
      " SMAUFNR, which differs from lv_order_number when SMAUFNR is empty.
      " Storno reads this value back, so they have to match.
      zcl_wr_pd_tour_helper=>log_bms_call(
        iv_tour_uuid    = ls_tour-touruuid
        iv_service_uuid = ls_asgmt-serviceuuid
        iv_order_number = lv_order_number
        iv_endpoint     = '/api/container/create-order-halle'
        iv_http_status  = lv_http_status
        iv_request      = lv_json
        iv_response     = lv_response
        iv_pobjnr       = ls_ewa-pobjnr ).

*----------------------------------------------------------------------*
* STEP 9 — Evaluate response
*----------------------------------------------------------------------*
      IF lv_http_status = 200 OR lv_http_status = 201.

        lv_count_ok = lv_count_ok + 1.

        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '021'
                   severity = if_abap_behv_message=>severity-success
                   v1       = lv_svc_ref_out
                   v2       = lv_order_number )
        ) TO reported-tour.

      ELSE.

        lv_count_error = lv_count_error + 1.

        " Show the real BMS reason. The old code discarded lv_response and
        " reported a bare "could not be transmitted" at severity
        " INFORMATION — which is what the cockpit screenshot shows.
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
                   number   = '022'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_svc_ref_out
                   v2       = lv_bms_msg ) ) TO reported-tour.

      ENDIF.

      " --- per-service status ------------------------------------------------
      DATA(lv_svc_bms_status) = COND string(
        WHEN lv_http_status = 200 OR lv_http_status = 201
        THEN 'FREIGEGEBEN'
        ELSE 'ERROR' ).

      CLEAR lv_upd_subrc.

      " EXCEPTIONS added — without them an RFC failure short-dumps instead
      " of setting sy-subrc. EV_SUBRC reports whether a row was actually
      " hit; the RFC's own sy-subrc never could, so the INSERT fallback
      " below was dead code.
      CALL FUNCTION 'ZWR_BMS_UPDATE_SERVICE'
        DESTINATION 'NONE'
        EXPORTING
          service_uuid          = ls_asgmt-serviceuuid
          zz_bms_status         = lv_svc_bms_status
        IMPORTING
          ev_subrc              = lv_upd_subrc
        EXCEPTIONS
          communication_failure = 1
          system_failure        = 2
          OTHERS                = 3.

      IF sy-subrc <> 0 OR lv_upd_subrc <> 0.
        CLEAR ls_srvcst.
        ls_srvcst-service_uuid  = ls_asgmt-serviceuuid.
        ls_srvcst-zz_bms_status = lv_svc_bms_status.
        INSERT /plce/tpdsrvcst FROM ls_srvcst.
      ENDIF.

    ENDLOOP.   " services

*----------------------------------------------------------------------*
* STEP 10 — Tour status
*           Derived from the in-memory counters. The old version re-read
*           /plce/tpdsrvcst per service, which depended on the RFC's LUW
*           having committed and turned a missing row into "ERROR".
*----------------------------------------------------------------------*
    DATA(lv_bms_status) = COND /plce/char20(
      WHEN lv_count_ok > 0 AND lv_count_error = 0 THEN 'SENT'
      WHEN lv_count_ok > 0                        THEN 'PARTIAL'
      ELSE                                             'ERROR' ).

    MODIFY ENTITIES OF /plce/r_pdtour IN LOCAL MODE
      ENTITY extcustom
        UPDATE FIELDS ( zz_bms_status )
        WITH VALUE #( ( touruuid      = ls_tour-touruuid
                        zz_bms_status = lv_bms_status ) )
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
                %cid          = |BMS_STAT_TGT_{ ls_tour-touruuid }|
                zz_bms_status = lv_bms_status ) ) ) )
        FAILED   DATA(lf_crt)
        REPORTED DATA(lr_crt).

      IF lf_crt IS NOT INITIAL.
        APPEND LINES OF lr_crt-tour TO reported-tour.
      ENDIF.
    ENDIF.

  ENDLOOP.   " tours

ENDMETHOD.


METHOD stornobmsservice.

*----------------------------------------------------------------------*
* TYPES / DATA
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
    lt_services    TYPE tt_svc_storno,
    ls_svc         TYPE ty_svc_storno,
    ls_srvcst      TYPE /plce/tpdsrvcst,
    lv_token       TYPE string,
    lv_error       TYPE string,
    lv_http_status TYPE i,
    lv_response    TYPE string,
    lv_storno_ok   TYPE i,
    lv_storno_err  TYPE i.

*----------------------------------------------------------------------*
* CONFIG
*----------------------------------------------------------------------*
  SELECT SINGLE bms_endpoint_url, bms_username, bms_password, active
    FROM ztour_bms_cfg
    WHERE config_id = 'DEFAULT'
    INTO @DATA(ls_cfg).

  IF sy-subrc <> 0 OR ls_cfg-active <> abap_true.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ky>).
      APPEND VALUE #(
        %tky = <ky>-%tky
        %msg = new_message(
                 id       = 'Z_MSG_SVR_TOUR_EXT'
                 number   = '018'
                 severity = if_abap_behv_message=>severity-error )
      ) TO reported-tour.
    ENDLOOP.
    RETURN.
  ENDIF.

  DATA(lv_bms_base_url) = ls_cfg-bms_endpoint_url.
  DATA(lv_bms_user)     = ls_cfg-bms_username.
  DATA(lv_bms_password) = ls_cfg-bms_password.

*----------------------------------------------------------------------*
* Tours and services
*----------------------------------------------------------------------*
  READ ENTITIES OF /plce/r_pdtour IN LOCAL MODE
    ENTITY tour
      FIELDS ( tourid touruuid )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tours)
    FAILED DATA(lt_failed).

  " same fix as in the release action — report and carry on
  LOOP AT lt_failed-tour ASSIGNING FIELD-SYMBOL(<fail>).
    APPEND VALUE #(
      %tky = <fail>-%tky
      %msg = new_message(
               id       = 'Z_MSG_SVR_TOUR_EXT'
               number   = '024'
               severity = if_abap_behv_message=>severity-error )
    ) TO reported-tour.
  ENDLOOP.

  IF lt_tours IS INITIAL.
    RETURN.
  ENDIF.

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
      WHERE touruuid = ls_tour-touruuid.

      CLEAR: ls_svc, lv_http_status, lv_response.

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

      " Take the order number from the log, i.e. the value that was really
      " sent. The old code re-derived it from SMAUFNR alone and therefore
      " tried to cancel an empty order number whenever SMAUFNR was blank.
      SELECT SINGLE order_number
        FROM zbms_api_log
        WHERE tour_uuid    = @ls_tour-touruuid
          AND service_uuid = @ls_asgmt-serviceuuid
        INTO @DATA(lv_order_number).

      IF sy-subrc <> 0 OR lv_order_number IS INITIAL.
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

      SELECT SINGLE pobjnr
        FROM ewa_order_object
        WHERE pobjnr = @ls_svc-reference_int_id
        INTO @DATA(lv_pobjnr).

      zcl_wr_pd_tour_helper=>storno_bms_order(
        EXPORTING
          iv_base_url     = lv_bms_base_url
          iv_bearer_token = lv_token
          iv_order_number = lv_order_number
          iv_full_json    = ''
        IMPORTING
          ev_http_status  = lv_http_status
          ev_response     = lv_response ).

      zcl_wr_pd_tour_helper=>log_bms_call(
        iv_tour_uuid    = ls_tour-touruuid
        iv_service_uuid = ls_asgmt-serviceuuid
        iv_order_number = lv_order_number
        iv_endpoint     = '/api/container/storno'
        iv_http_status  = lv_http_status
        iv_request      = |STORNO: { lv_order_number }|
        iv_response     = lv_response
        iv_pobjnr       = lv_pobjnr ).

      IF lv_http_status = 200 OR lv_http_status = 201.

        lv_storno_ok = lv_storno_ok + 1.

        UPDATE /plce/tpdsrvcst
          SET zz_bms_status = 'STORNIERT'
          WHERE service_uuid = @ls_asgmt-serviceuuid.

        IF sy-subrc <> 0.
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

        APPEND VALUE #(
          %tky = ls_tour-%tky
          %msg = new_message(
                   id       = 'Z_MSG_SVR_TOUR_EXT'
                   number   = '016'
                   severity = if_abap_behv_message=>severity-error
                   v1       = lv_order_number
                   v2       = lv_response )
        ) TO reported-tour.

      ENDIF.

    ENDLOOP.   " services

    " The old code set STORNIERT unconditionally — a tour whose every
    " cancellation failed still showed as cancelled while the orders were
    " still on the drivers' tablets.
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

