METHOD anlageaendern.

  DATA: lt_ext_create TYPE TABLE FOR CREATE /plce/r_pdservice\_extcustom,
        lt_ext_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extcustom,
        lt_extwr_update TYPE TABLE FOR UPDATE /plce/r_pdservice\\extwr.

  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service
    FIELDS ( serviceuuid referenceinternalid referenceid )
    WITH CORRESPONDING #( keys )
    RESULT DATA(lt_services).

  LOOP AT lt_services ASSIGNING FIELD-SYMBOL(<ls_service>).

    DATA(ls_param) = keys[ %tky = <ls_service>-%tky ]-%param.

    " ✅ Get ALL needed data from order object + facility
    SELECT SINGLE o~zz_tech_fachbe,
                  o~zz_pobjnr_main,
                  f~tplnr
      FROM zi_wr_ewa_order_object AS o
      LEFT OUTER JOIN ewa_el_wdplant AS f
        ON f~wdplantnr = @ls_param-anlage
      WHERE o~pobjnr = @<ls_service>-referenceinternalid
      INTO @DATA(ls_data).

    " Update ExtWR plant_location
    IF ls_param-anlage IS NOT INITIAL AND ls_data-tplnr IS NOT INITIAL.
      APPEND VALUE #(
        %key-serviceuuid = <ls_service>-serviceuuid
        plantlocation = ls_data-tplnr
      ) TO lt_extwr_update.
    ELSEIF ls_param-anlage IS INITIAL.
      " Clear plant_location if facility is cleared
      APPEND VALUE #(
        %key-serviceuuid = <ls_service>-serviceuuid
        plantlocation = ''
      ) TO lt_extwr_update.
    ENDIF.

    " Check if ExtCustom exists
    READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      FIELDS ( serviceuuid wdplantnr zz_tech_fachbe zz_pobjnr_main )
      WITH VALUE #( ( serviceuuid = <ls_service>-serviceuuid ) )
      RESULT DATA(lt_extcustom).

    IF lt_extcustom IS INITIAL.
      " ✅ Create ExtCustom with ALL fields
      DATA(lv_cid) = |$cid_{ <ls_service>-serviceuuid }$|.

      APPEND VALUE #( 
        serviceuuid = <ls_service>-serviceuuid
        %target = VALUE #( ( 
          %cid = lv_cid
          wdplantnr = ls_param-anlage
          zz_tech_fachbe = ls_data-zz_tech_fachbe
          zz_pobjnr_main = ls_data-zz_pobjnr_main
        ) )
      ) TO lt_ext_create.

    ELSE.
      " ✅ Update ExtCustom: always update wdplantnr, populate empty fields
      DATA(ls_ext) = lt_extcustom[ 1 ].

      APPEND VALUE #( 
        %key-serviceuuid = <ls_service>-serviceuuid
        wdplantnr = ls_param-anlage  " User's selection
        " Populate fachbereich if empty
        zz_tech_fachbe = COND #( WHEN ls_ext-zz_tech_fachbe IS INITIAL
                                 THEN ls_data-zz_tech_fachbe
                                 ELSE ls_ext-zz_tech_fachbe )
        " Populate hauptposition if empty
        zz_pobjnr_main = COND #( WHEN ls_ext-zz_pobjnr_main IS INITIAL
                                 THEN ls_data-zz_pobjnr_main
                                 ELSE ls_ext-zz_pobjnr_main )
      ) TO lt_ext_update.
    ENDIF.

    CLEAR: lt_extcustom, ls_data.

  ENDLOOP.

  " Bulk update ExtWR
  IF lt_extwr_update IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extwr
      UPDATE FIELDS ( plantlocation )
      WITH lt_extwr_update.
  ENDIF.

  " Bulk create ExtCustom
  IF lt_ext_create IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY service
      CREATE BY \_extcustom
      AUTO FILL CID SET FIELDS WITH lt_ext_create
      MAPPED DATA(lmapped)
      FAILED DATA(lfailed)
      REPORTED DATA(lreported).

    IF lfailed IS NOT INITIAL.
      APPEND LINES OF lfailed-extcustom TO failed-extcustom.
      APPEND LINES OF lreported-extcustom TO reported-extcustom.
    ENDIF.
  ENDIF.

  " Bulk update ExtCustom
  IF lt_ext_update IS NOT INITIAL.
    MODIFY ENTITIES OF /plce/r_pdservice IN LOCAL MODE
      ENTITY extcustom
      UPDATE FIELDS ( wdplantnr zz_tech_fachbe zz_pobjnr_main )  " ✅ Update all 3
      WITH lt_ext_update
      FAILED DATA(lt_failed_update)
      REPORTED DATA(lt_reported_update).

    IF lt_failed_update IS NOT INITIAL.
      APPEND LINES OF lt_failed_update-extcustom TO failed-extcustom.
      APPEND LINES OF lt_reported_update-extcustom TO reported-extcustom.
    ELSE.
      " Success messages
      LOOP AT lt_services ASSIGNING <ls_service>.
        DATA(ls_param_msg) = keys[ %tky = <ls_service>-%tky ]-%param.

        IF ls_param_msg-anlage IS NOT INITIAL.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '015'
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = <ls_service>-referenceid )
                        ) TO reported-service.
        ELSE.
          APPEND VALUE #( %tky = <ls_service>-%tky
                          %msg = new_message(
                                   id       = 'Z_MSG_SVR_TOUR_EXT'
                                   number   = '016'
                                   severity = if_abap_behv_message=>severity-information
                                   v1       = <ls_service>-referenceid )
                        ) TO reported-service.
        ENDIF.
      ENDLOOP.
    ENDIF.
  ENDIF.

  " Return updated data
  READ ENTITIES OF /plce/r_pdservice IN LOCAL MODE
    ENTITY service ALL FIELDS WITH CORRESPONDING #( keys ) RESULT DATA(lt_service_result).

  result = VALUE #( FOR srv IN lt_service_result ( %tky = srv-%tky %param = srv ) ).

ENDMETHOD.
