CLASS zcl_weighing_handler DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    CLASS-METHODS cl_weighing
      IMPORTING
        iv_contractid     TYPE vbeln_va
        iv_wasteid        TYPE waste_type OPTIONAL
        iv_licence_plate  TYPE license_num OPTIONAL
        iv_simulation     TYPE abap_bool DEFAULT abap_false
        iv_user           TYPE xubname
        iv_commit         TYPE abap_bool OPTIONAL
        iv_deliverytype   TYPE ewadwa_weighprocintext DEFAULT 'I'
        iv_wdplant        TYPE ewael_wdplantnr OPTIONAL
        iv_devicegroupnr  TYPE ewawa_devicegroupnr OPTIONAL
      EXPORTING
        iv_weighing_result TYPE zwr_swaa_selfweigh_result
      RAISING
        cx_eewa_base.

    CLASS-METHODS get_open_weighproc
      IMPORTING
        iv_contractid     TYPE vbeln_va
      RETURNING
        VALUE(rv_weighingid) TYPE ewawa_weighingnr.

  PROTECTED SECTION.

  PRIVATE SECTION.
    TYPES:
      ty_weighing_request TYPE zcl_wr_waa_ta_selfweighing=>sty_weighing_request,
      ty_weighing_result  TYPE zwr_swaa_selfweigh_result,
      ty_hw_profile       TYPE eewa_scale_user.

    CLASS-METHODS get_hw_profile
      IMPORTING
        iv_user           TYPE xubname
      RETURNING
        VALUE(rs_profile) TYPE ty_hw_profile.

    CLASS-METHODS execute_transaction
      IMPORTING
        is_process       TYPE ty_weighing_request
        iv_commit        TYPE abap_bool
      RETURNING
        VALUE(rs_result) TYPE ty_weighing_result
      RAISING
        cx_eewa_base.

ENDCLASS.

CLASS zcl_weighing_handler IMPLEMENTATION.

  METHOD cl_weighing.
    DATA: ls_process TYPE ty_weighing_request,
          lv_commit  TYPE abap_bool.

    CLEAR iv_weighing_result.

    ls_process-devicegroup = iv_devicegroupnr.
    ls_process-facilityid  = iv_wdplant.

    IF ls_process-devicegroup IS INITIAL.
      DATA(ls_hw_profile) = get_hw_profile( iv_user ).
      IF ls_hw_profile IS NOT INITIAL.
        ls_process-devicegroup = ls_hw_profile-devicegroupnr.
        ls_process-facilityid  = ls_hw_profile-wdplant.
      ENDIF.
    ENDIF.

    IF ls_process-devicegroup IS NOT INITIAL.
      ls_process-weighingid   = get_open_weighproc( iv_contractid ).
      ls_process-processtype  = cl_eewa_bo_wdorderweigh=>cprocesstype_inbound.
      ls_process-simulation   = iv_simulation.
      ls_process-wasteid      = iv_wasteid.
      ls_process-deliverytype = iv_deliverytype.
      " ls_process-licence_plate = iv_licence_plate.  " Uncomment and adjust if field exists in ty_weighing_request

      lv_commit = iv_commit.  " Optional, defaults to ABAP_FALSE if not provided

      iv_weighing_result = execute_transaction(
                             is_process = ls_process
                             iv_commit  = lv_commit ).
    ENDIF.
  ENDMETHOD.

  METHOD get_hw_profile.
    SELECT SINGLE *
      FROM eewa_scale_user
      INTO @rs_profile
      WHERE xuser = @iv_user.
  ENDMETHOD.

  METHOD get_open_weighproc.
    TYPES:
      BEGIN OF ty_weighinglead,
        weighingnr_lead TYPE ewa_wa_weighproc-weighingnr_lead,
      END OF ty_weighinglead.

    DATA:
      lr_ranges     TYPE REF TO ewapranges,
      lt_weighingnr TYPE STANDARD TABLE OF ty_weighinglead,
      lv_max        TYPE int4 VALUE 1,
      lv_grossid    TYPE ewa_wa_weighproc-gross_id,
      lv_tareid     TYPE ewa_wa_weighproc-tare_id,
      lv_weighinglead TYPE ewa_wa_weighproc-weighingnr_lead.

    IF iv_contractid IS INITIAL.
      RETURN.
    ENDIF.

    CREATE DATA lr_ranges.
    FIELD-SYMBOLS: <lt_ranges> TYPE ewapranges.
    ASSIGN lr_ranges->* TO <lt_ranges>.

    " For CONTRACT_VBELN
    TYPES tt_vbeln_range TYPE RANGE OF vbeln_va.
    DATA(lt_contract_range) = VALUE tt_vbeln_range( ( sign = 'I' option = 'EQ' low = iv_contractid ) ).
    DATA(lr_contract_ref) = REF #( lt_contract_range ).
    APPEND VALUE #( name = 'CONTRACT_VBELN' range = lr_contract_ref ) TO <lt_ranges>.

    " For GROSS_ID
    TYPES tt_gross_range TYPE RANGE OF ewa_wa_weighproc-gross_id.
    DATA(lt_gross_range) = VALUE tt_gross_range( ( sign = 'I' option = 'NE' low = lv_grossid ) ).
    DATA(lr_gross_ref) = REF #( lt_gross_range ).
    APPEND VALUE #( name = 'GROSS_ID' range = lr_gross_ref ) TO <lt_ranges>.

    " For TAREID
    TYPES tt_tare_range TYPE RANGE OF ewa_wa_weighproc-tare_id.
    DATA(lt_tare_range) = VALUE tt_tare_range( ( sign = 'I' option = 'EQ' low = lv_tareid ) ).
    DATA(lr_tare_ref) = REF #( lt_tare_range ).
    APPEND VALUE #( name = 'TAREID' range = lr_tare_ref ) TO <lt_ranges>.

    " For WEIGHINGNR_LEAD
    TYPES tt_lead_range TYPE RANGE OF ewa_wa_weighproc-weighingnr_lead.
    DATA(lt_lead_range) = VALUE tt_lead_range( ( sign = 'I' option = 'NE' low = lv_weighinglead ) ).
    DATA(lr_lead_ref) = REF #( lt_lead_range ).
    APPEND VALUE #( name = 'WEIGHINGNR_LEAD' range = lr_lead_ref ) TO <lt_ranges>.

    cl_eewa_or_weighingprocess=>cl_read(
      EXPORTING
        par_objtype = cl_eewa_bo_weighingprocess=>cot_weighingprocess
        par_domain  = 'OPEN'
        par_ranges  = lr_ranges
        par_maxrows = lv_max
      IMPORTING
        par_table   = lt_weighingnr
    ).

    IF line_exists( lt_weighingnr[ 1 ] ).
      rv_weighingid = lt_weighingnr[ 1 ]-weighingnr_lead.
    ENDIF.
  ENDMETHOD.

  METHOD execute_transaction.
    DATA(lo_ta) = NEW zcl_wr_waa_ta_selfweighing( ).

    lo_ta->gs_process_data = is_process.

    TRY.
        lo_ta->zz_book_weighing( ).
      CATCH cx_eewa_base INTO DATA(lx_error).
        ROLLBACK WORK.
        RAISE EXCEPTION lx_error.
    ENDTRY.

    rs_result = lo_ta->gs_result_data.

    IF iv_commit = abap_true.
      COMMIT WORK AND WAIT.
    ELSE.
      " No commit; changes may be buffered or not persisted depending on BO implementation
      " If needed, add ROLLBACK WORK here for simulation/testing, but only if updates are in buffer
    ENDIF.
  ENDMETHOD.

ENDCLASS.
