METHOD do_prepare_output.

  DATA: lcc         TYPE REF TO cl_ewa_ic_o_cc_impl,
        lbolpartner TYPE REF TO if_bol_bo_property_access,
        lpartner    TYPE        bu_partner,
        lt_hday     TYPE STANDARD TABLE OF zwr_s_hday_log.

  CALL METHOD super->do_prepare_output
    EXPORTING
      iv_first_time = iv_first_time.

* Kein add_buttons( ) - der View hat bewusst keine Toolbar

  lcc ?= get_custom_controller( controller_id = 'EWA_IC_OV_ACCT/CC' ).
  CHECK lcc IS NOT INITIAL.
  lcc->fill_from_ic( ).
  lbolpartner = lcc->typed_context->partner->collection_wrapper->get_current( ).

  IF lbolpartner IS NOT INITIAL.
    lbolpartner->get_property_as_value(
      EXPORTING
        iv_attr_name = 'PARTNER'
      IMPORTING
        ev_result    = lpartner ).
  ENDIF.

  CHECK lpartner IS NOT INITIAL.

* AK 1 + AK 2: ausschliesslich Urlaubstage des aktuellen Kunden
  SELECT * FROM zwr_ctp_op_hday
    INTO CORRESPONDING FIELDS OF TABLE @lt_hday
    WHERE partner EQ @lpartner.

* AK 3: Gueltigkeit absteigend, neueste zuerst
  SORT lt_hday BY start_date DESCENDING
                  end_date   DESCENDING.

  DATA(lr_tools_bol) = NEW zcrm_ui_tools_bol( ).
  DATA(lr_col) = lr_tools_bol->get_collection_from_table( it_table = lt_hday ).
  me->typed_context->holidaylog->set_collection( lr_col ).

ENDMETHOD.
