REPORT z_test_populate_extcustom.

START-OF-SELECTION.

  DATA: lt_keys TYPE TABLE FOR DETERMINATION /plce/r_pdservice~populateextcustomfromorder.

  " Add the problem service
  APPEND VALUE #( serviceuuid = 'FB88F7092F1B1FE192E528' ) TO lt_keys.

  " Manually trigger the determination
  DATA(lo_handler) = NEW lhc_service( ).
  lo_handler->populateextcustomfromorder( lt_keys ).

  " Check the result
  SELECT SINGLE wdplantnr, zz_tech_fachbe, zz_pobjnr_main
    FROM /plce/tpdsrvcst
    WHERE service_uuid = 'FB88F7092F1B1FE192E528'
    INTO @DATA(ls_result).

  IF sy-subrc = 0.
    WRITE: / 'After determination:'.
    WRITE: / 'wdplantnr =', ls_result-wdplantnr.
    WRITE: / 'fachbereich =', ls_result-zz_tech_fachbe.
    WRITE: / 'hauptposition =', ls_result-zz_pobjnr_main.
  ELSE.
    WRITE: / 'ExtCustom record not found!'.
  ENDIF.
