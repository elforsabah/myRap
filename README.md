CLASS zcl_wr_weighbridge_helper DEFINITION
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
        iv_user           TYPE xubname   " DEFAULT sy-uname not allowed in definition; handle in body if needed
        iv_commit         TYPE abap_bool OPTIONAL
        iv_deliverytype   TYPE ewadwa_weighprocintext DEFAULT 'I'
        iv_wdplant        TYPE ewael_wdplantnr OPTIONAL
        iv_devicegroupnr  TYPE ewawa_devicegroupnr OPTIONAL
      EXPORTING
        iv_weighing_result TYPE zwr_swaa_selfweigh_result
      RAISING
        cx_eewa_base.

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

    CLASS-METHODS get_open_weighproc
      IMPORTING
        iv_contractid     TYPE vbeln_va
      RETURNING
        VALUE(rv_weighingid) TYPE eewa_weighingid.

    CLASS-METHODS execute_transaction
      IMPORTING
        is_process       TYPE ty_weighing_request
        iv_commit        TYPE abap_bool
      RETURNING
        VALUE(rs_result) TYPE ty_weighing_result
      RAISING
        cx_eewa_base.

ENDCLASS.

CLASS zcl_wr_weighbridge_helper IMPLEMENTATION.

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
      ls_process-licence_plate = iv_licence_plate.  " Assuming the field exists in ty_weighing_request

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

    " No exception; original skips if not found
  ENDMETHOD.

  METHOD get_open_weighproc.
    rv_weighingid = cl_get_open_weighproc(
                      EXPORTING
                        iv_contract = iv_contractid ).

    " Assume it may raise internally if needed; no explicit check
  ENDMETHOD.

  METHOD execute_transaction.
    " Assuming a modern transaction automation class replaces the macros.
    " Here, we instantiate an object for transaction handling.
    DATA(lo_ta) = NEW zcl_transaction_automation(  " Hypothetical modern TA class
                            name            = zcl_wr_waa_ta_selfweighing=>c_ta_name
                            raise_exception = abap_true
                            do_commit       = iv_commit ).

    lo_ta->set_param( gs_process_data = is_process ).
    lo_ta->execute( 'ZZ_BOOK_WEIGHING' ).
    lo_ta->get_param( IMPORTING gs_result_data = rs_result ).

    " Handle any transaction errors via exceptions in the TA class (e.g., raises CX_EEWA_BASE)
  ENDMETHOD.

ENDCLASS.
