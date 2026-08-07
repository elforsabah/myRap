  METHOD do_prepare_output.

    DATA: lcc         TYPE REF TO cl_ewa_ic_o_cc_impl,
          lbolpartner TYPE REF TO if_bol_bo_property_access,
          lpartner    TYPE        bu_partner,
          lkeydate    TYPE        dats.

    CALL METHOD super->do_prepare_output
      EXPORTING
        iv_first_time = iv_first_time.

    IF iv_first_time IS NOT INITIAL.
      add_buttons( ).
    ENDIF.

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

    IF lpartner IS NOT INITIAL.
      SELECT * FROM qmel
        INTO TABLE @DATA(lt_qmel)
        WHERE kunum EQ @lpartner
          AND qmart EQ 'Q1'.
      IF lt_qmel IS NOT INITIAL.
        DATA(lr_tools_bol) = NEW zcrm_ui_tools_bol( ).
        DATA(lr_col) = lr_tools_bol->get_collection_from_table( it_table = lt_qmel ).
        me->typed_context->qmeldata->set_collection( lr_col ).
      ENDIF.
    ENDIF.

  ENDMETHOD.


    method DO_INIT_CONTEXT.

*   set initial selection mode for all tables



    TYPED_CONTEXT->QMELDATA->SET_SELECTION_MODE(
      IV_SELECTION_MODE = CL_BSP_WD_CONTEXT_NODE_TV=>SELMODE_SINGLE
    ).



  endmethod.
