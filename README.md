METHOD stornoBMSService.

  CONSTANTS:
    lc_bms_base_url TYPE string
      VALUE 'http://hws-srv17.hws.vvv.vvv-konzern.net/BmsApiSapTest',
    lc_bms_user     TYPE string VALUE 'BmsWebApi',
    lc_bms_password TYPE string VALUE 'uh}O1Nm#&lV+2LeS'.

  " Read the services to be cancelled — passed via action parameter
  " keys contains either TourUUID (cancel whole tour)
  " or individual ServiceUUIDs from the parameter
  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour
      FIELDS ( TourId TourUUID )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_tours)
    FAILED DATA(lt_failed).

  CHECK lt_failed IS INITIAL.

  " Get all service assignments for the selected tours
  READ ENTITIES OF /PLCE/R_PDTour IN LOCAL MODE
    ENTITY Tour BY \_ServiceAssignments
      FIELDS ( TourUUID ServiceUUID Removed )
      WITH CORRESPONDING #( keys )
    RESULT DATA(lt_asgmts).

  DELETE lt_asgmts WHERE Removed IS NOT INITIAL.

  " Read EWA order numbers via service base table
  DATA lt_svc_uuids TYPE RANGE OF /plce/pdservice_uuid.
  lt_svc_uuids = VALUE #(
    FOR ls IN lt_asgmts
    ( sign = 'I' option = 'EQ' low = ls-ServiceUUID ) ).

  DATA lt_services TYPE HASHED TABLE OF /plce/tpdsrv
                   WITH UNIQUE KEY service_uuid.

  IF lt_svc_uuids IS NOT INITIAL.
    SELECT service_uuid, reference_id, reference_int_id AS reference_internal_id
      FROM /plce/tpdsrv
      WHERE service_uuid IN @lt_svc_uuids
      INTO CORRESPONDING FIELDS OF TABLE @lt_services.
  ENDIF.

  LOOP AT lt_tours INTO DATA(ls_tour).

    " Auth once per tour
    DATA lv_token TYPE string.
    DATA lv_error TYPE string.

    zcl_wr_pd_tour_helper=>get_bms_bearer_token(
      EXPORTING
        iv_base_url = lc_bms_base_url
        iv_username = lc_bms_user
        iv_password = lc_bms_password
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

      DATA ls_svc TYPE /plce/tpdsrv.
      READ TABLE lt_services INTO ls_svc
        WITH KEY service_uuid = ls_asgmt-ServiceUUID.
      CHECK sy-subrc = 0.

      " Read order number from EWA
      DATA ls_ewa TYPE ewa_order_object.
      SELECT SINGLE smaufnr FROM ewa_order_object
        WHERE pobjnr = @ls_svc-reference_int_id
        INTO @DATA(lv_smaufnr).

      IF sy-subrc <> 0.
        CONTINUE.
      ENDIF.

      DATA lv_status   TYPE i.
      DATA lv_response TYPE string.

      zcl_wr_pd_tour_helper=>storno_bms_order(
        EXPORTING
          iv_base_url     = lc_bms_base_url
          iv_bearer_token = lv_token
          iv_order_number = |{ lv_smaufnr ALPHA = OUT }|
          iv_full_json    = ''   " empty = minimal storno
        IMPORTING
          ev_http_status  = lv_status
          ev_response     = lv_response ).

      " Log every storno call
      zcl_wr_pd_tour_helper=>log_bms_call(
        iv_tour_uuid    = ls_tour-TourUUID
        iv_service_uuid = ls_asgmt-ServiceUUID
        iv_order_number = lv_smaufnr
        iv_endpoint     = '/api/container/create-order-halle (STORNO)'
        iv_http_status  = lv_status
        iv_request      = |STORNIERT: { lv_smaufnr ALPHA = OUT }|
        iv_response     = lv_response ).

      IF lv_status = 200 OR lv_status = 201.
        " Update service BMS status to STORNIERT
        MODIFY ENTITIES OF /PLCE/R_PDService IN LOCAL MODE
          ENTITY Service BY \_ExtCustom
            UPDATE FIELDS ( zz_bms_status )
            WITH VALUE #(
              ( ServiceUUID   = ls_asgmt-ServiceUUID
                zz_bms_status = 'STORNIERT' ) )
          FAILED   DATA(lf_s)
          REPORTED DATA(lr_s).

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
  ENDLOOP.

ENDMETHOD.
