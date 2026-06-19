METHOD stornoBMSService.

  "=======================================================================
  " CONFIG — read from ZTOUR_BMS_CFG (same as touranBMSfreigeben)
  "=======================================================================
  SELECT SINGLE bms_endpoint_url,
                bms_username,
                bms_password,
                active
    FROM ztour_bms_cfg
    WHERE mandt = @sy-mandt
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
          ls_srvcst-mandt         = sy-mandt.
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

ENDMETHOD.
