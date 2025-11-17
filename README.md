CLASS zcl_weighing_handler DEFINITION
  PUBLIC
  FINAL
  CREATE PUBLIC.

  PUBLIC SECTION.
    TYPES:
      ty_weighing_request TYPE zcl_wr_waa_ta_selfweighing=>sty_weighing_request,
      ty_weighing_result  TYPE zwr_swaa_selfweigh_result,
      ty_hw_profile       TYPE eewa_scale_user.

    METHODS cl_weighing
      IMPORTING
        iv_devicegroupnr TYPE eewa_devicegroupnr OPTIONAL
        iv_wdplant       TYPE eewa_wdplant OPTIONAL
        iv_user          TYPE xuser
        iv_contractid    TYPE eewa_contractid
        iv_simulation    TYPE abap_bool DEFAULT abap_false
        iv_wasteid       TYPE eewa_wasteid OPTIONAL
        iv_commit        TYPE abap_bool DEFAULT abap_true
      EXPORTING
        ev_weighing_result TYPE ty_weighing_result
      RAISING
        zcx_weighing_error.  " Custom exception class for errors

  PROTECTED SECTION.

  PRIVATE SECTION.
    METHODS get_hw_profile
      IMPORTING
        iv_user          TYPE xuser
      RETURNING
        VALUE(rs_profile) TYPE ty_hw_profile
      RAISING
        zcx_weighing_error.

    METHODS get_open_weighproc
      IMPORTING
        iv_contractid     TYPE eewa_contractid
      RETURNING
        VALUE(rv_weighingid) TYPE eewa_weighingid
      RAISING
        zcx_weighing_error.

    METHODS execute_transaction
      IMPORTING
        is_process       TYPE ty_weighing_request
        iv_commit        TYPE abap_bool
      RETURNING
        VALUE(rs_result) TYPE ty_weighing_result
      RAISING
        zcx_weighing_error.

ENDCLASS.

CLASS zcl_weighing_handler IMPLEMENTATION.

  METHOD cl_weighing.
    DATA: ls_process TYPE ty_weighing_request.

    CLEAR ev_weighing_result.

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
      ls_process-deliverytype = cl_eewa_bo_multiweighprochead=>cdt_external.

      ev_weighing_result = execute_transaction(
                             is_process = ls_process
                             iv_commit  = iv_commit ).
    ELSE.
      RAISE EXCEPTION TYPE zcx_weighing_error
        EXPORTING
          textid = zcx_weighing_error=>devicegroup_missing.
    ENDIF.
  ENDMETHOD.

  METHOD get_hw_profile.
    SELECT SINGLE *
      FROM eewa_scale_user
      INTO @rs_profile
      WHERE xuser = @iv_user.

    IF sy-subrc <> 0.
      RAISE EXCEPTION TYPE zcx_weighing_error
        EXPORTING
          textid = zcx_weighing_error=>profile_not_found.
    ENDIF.
  ENDMETHOD.

  METHOD get_open_weighproc.
    rv_weighingid = cl_get_open_weighproc(
                      EXPORTING
                        iv_contract = iv_contractid ).

    IF rv_weighingid IS INITIAL.
      RAISE EXCEPTION TYPE zcx_weighing_error
        EXPORTING
          textid = zcx_weighing_error=>no_open_weighproc.
    ENDIF.
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

    " Handle any transaction errors via exceptions in the TA class.
  ENDMETHOD.

ENDCLASS.
