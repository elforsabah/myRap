class ZCL_WR_CCMG_MISC definition
  public
  create public .

public section.

  class-methods ZCL_HAS_ERRORS
    importing
      !IO_GLOBAL_MESSAGES type ref to CL_BSP_MESSAGES
    returning
      value(IR_RESULT) type ABAP_BOOL .
protected section.
private section.
ENDCLASS.



CLASS ZCL_WR_CCMG_MISC IMPLEMENTATION.


  METHOD ZCL_HAS_ERRORS.
    IR_RESULT = ABAP_TRUE.
    DATA(LV_MESSAGES_NUM) = IO_GLOBAL_MESSAGES->NUM_MESSAGES( ).
    DO LV_MESSAGES_NUM TIMES.
      IO_GLOBAL_MESSAGES->GET_MESSAGE( EXPORTING INDEX    = SY-INDEX
                                       IMPORTING SEVERITY = DATA(LV_SEVERITY) ).
      IF LV_SEVERITY = CL_BSP_MESSAGES=>CO_SEVERITY_ERROR
      OR LV_SEVERITY = CL_BSP_MESSAGES=>CO_SEVERITY_FATAL_ERROR.
        IR_RESULT = ABAP_TRUE.
        EXIT.
      ENDIF.
    ENDDO.
  ENDMETHOD.
ENDCLASS.

class ZL_CN_ZWRELOCSD_NEW definition
  public
  inheriting from CL_BSP_WD_CONTEXT_NODE
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW' ##NO_TEXT.

  methods GET_CONTSHARE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_CONT_COUNT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_I_AB
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_BIS
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_CONTSHARE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_CONT_COUNT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_EQUNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_IBASE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ILOAN
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_KUNNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_LOGIKNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_MATNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_OBKNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_OBZAE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_POSNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_SERVFREQNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_SERVLOC
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_TIMFR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_TIMTO
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_VBELN
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZADRUNCANR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZADRVPGRPNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZAVVCODE_DEF
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZBILL_BLOCK
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZCONTASSIGNMENT_ID
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZINVOICE_SPLIT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZNO_PORTAL
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZPO_MANDAT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZREQUESTER_MANDAT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZSALES_PERSON
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZWASTE_DEF
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZWASTE_DEF_DESCR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_M_CONTSHARE
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_CONT_COUNT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZADRUNCANR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZADRVPGRPNR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZAVVCODE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZBILL_BLOCK
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZCONTASSIGNMENT_ID
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZINVOICE_SPLIT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZNO_PORTAL
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZPO_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZREQUESTER_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZSALES_PERSON
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZWASTE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZWASTE_DEF_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_P_CONTSHARE
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_CONT_COUNT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZAVVCODE_DEF
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZBILL_BLOCK
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZINVOICE_SPLIT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZNO_PORTAL
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZPO_MANDAT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZREQUESTER_MANDAT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZSERVTYPE_DEF
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_V_ZZAVVCODE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZINVOICE_SPLIT
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZROUTE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZSERVTYPE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZWASTE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZWDF_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_ZZADRUNCANR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZADRVPGRPNR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZAVVCODE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZBILL_BLOCK
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZCONTASSIGNMENT_ID
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZINVOICE_SPLIT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZNO_PORTAL
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZPO_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZREQUESTER_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZSALES_PERSON
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZWASTE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZWASTE_DEF_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods SET_CONTSHARE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_CONT_COUNT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZAVVCODE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZBILL_BLOCK
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZINVOICE_SPLIT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZNO_PORTAL
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZPO_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZREQUESTER_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZSALES_PERSON
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZWASTE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
protected section.

  data FVH_ZZINVOICE_SPLIT type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  data FVH_ZZSERVTYPE_DEF type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
private section.
ENDCLASS.



CLASS ZL_CN_ZWRELOCSD_NEW IMPLEMENTATION.


  method GET_CONTSHARE.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'CONTSHARE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/CONTSHARE not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_CONT_COUNT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'CONT_COUNT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/CONT_COUNT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


method GET_I_AB.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_BIS.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_CONTSHARE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_CONT_COUNT.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_EQUNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_IBASE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ILOAN.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_KUNNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_LOGIKNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_MATNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_OBKNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_OBZAE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_POSNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_SERVFREQNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_SERVLOC.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_TIMFR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_TIMTO.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_VBELN.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ZZADRUNCANR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ZZADRVPGRPNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ZZAVVCODE_DEF.
  data: CURRENT type ref to IF_BOL_BO_PROPERTY_ACCESS.

  RV_DISABLED = 'TRUE'.
  if ITERATOR is bound.
    CURRENT = ITERATOR->GET_CURRENT( ).
  else.
    CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
  endif.

  try.

      if CURRENT->IS_PROPERTY_READONLY(
                    'ZZAVVCODE_DEF' ) = ABAP_FALSE.         "#EC NOTEXT
        RV_DISABLED = 'FALSE'.
      endif.

    catch CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
          CX_CRM_GENIL_MODEL_ERROR.
      return.
  endtry.



endmethod.


  method GET_I_ZZBILL_BLOCK.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZBILL_BLOCK' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


method GET_I_ZZCONTASSIGNMENT_ID.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


  method GET_I_ZZINVOICE_SPLIT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZINVOICE_SPLIT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZNO_PORTAL.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZNO_PORTAL' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZPO_MANDAT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZPO_MANDAT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZREQUESTER_MANDAT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZREQUESTER_MANDAT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZSALES_PERSON.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZSALES_PERSON' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZWASTE_DEF.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZWASTE_DEF' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


method GET_I_ZZWASTE_DEF_DESCR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


  method GET_M_CONTSHARE.

  DATA: attr    TYPE string.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'CONTSHARE'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_CONT_COUNT.

  DATA: attr    TYPE NUMC4.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'CONT_COUNT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZADRUNCANR.

  DATA: attr    TYPE /WATP/DADRUNCANR.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZADRUNCANR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZADRVPGRPNR.

  DATA: attr    TYPE /WATP/DADRVPGRPNR.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZADRVPGRPNR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZAVVCODE_DEF.

  DATA: attr    TYPE /WATP/DAVVCODE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZAVVCODE_DEF'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZBILL_BLOCK.

  DATA: attr    TYPE ZWR_DCMGM_BILL_BLOCK.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZBILL_BLOCK'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZCONTASSIGNMENT_ID.

  DATA: attr    TYPE ZWR_DCMGM_CONTASSIGNMENT_ID.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZCONTASSIGNMENT_ID'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZINVOICE_SPLIT.

  DATA: attr    TYPE ZWR_DCMGM_INVOICE_SPLIT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZINVOICE_SPLIT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZNO_PORTAL.

  DATA: attr    TYPE ZWR_DCMGM_NO_PORTAL.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZNO_PORTAL'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZPO_MANDAT.

  DATA: attr    TYPE ZWR_DCMGM_PO_MANDAT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZPO_MANDAT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZREQUESTER_MANDAT.

  DATA: attr    TYPE ZWR_DCMGM_REQUESTER_MANDAT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZREQUESTER_MANDAT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZSALES_PERSON.

  DATA: attr    TYPE ZWR_DCMGM_SALES_PERSON.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZSALES_PERSON'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZWASTE_DEF.

  DATA: attr    TYPE WASTE_TYPE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZWASTE_DEF'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZWASTE_DEF_DESCR.

  DATA: attr    TYPE MAKTX.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZWASTE_DEF_DESCR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


method GET_P_CONTSHARE.
  case IV_PROPERTY .
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_FIELDTYPE.
      RV_VALUE = CL_BSP_DLC_VIEW_DESCRIPTOR=>FIELD_TYPE_INPUT.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_TEXT_DIRECTION.
      RV_VALUE = CL_THTMLB_COMMON_CONSTANT=>GC_TEXTDIRECTION_LTR.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_CUSTOM_CSS.
      RV_VALUE = 'th-tx-l' ##NO_TEXT.
  endcase.
endmethod.


method GET_P_CONT_COUNT.
  case IV_PROPERTY .
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_FIELDTYPE.
      RV_VALUE = CL_BSP_DLC_VIEW_DESCRIPTOR=>FIELD_TYPE_INPUT.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_TEXT_DIRECTION.
      RV_VALUE = CL_THTMLB_COMMON_CONSTANT=>GC_TEXTDIRECTION_LTR.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_CUSTOM_CSS.
      RV_VALUE = 'th-tx-l' ##NO_TEXT.
  endcase.
endmethod.


METHOD GET_P_ZZAVVCODE_DEF.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      "rv_value = cl_bsp_dlc_view_descriptor=>field_type_input.

    "WHEN IF_BSP_WD_MODEL_SETTER_GETTER=>...
      "...
  ENDCASE.

ENDMETHOD.


method GET_P_ZZBILL_BLOCK.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


method GET_P_ZZINVOICE_SPLIT.
  VIEW_CONTEXT_PROP_PICKLIST.
endmethod.


method GET_P_ZZNO_PORTAL.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


method GET_P_ZZPO_MANDAT.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


method GET_P_ZZREQUESTER_MANDAT.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


METHOD GET_P_ZZSERVTYPE_DEF.
  VIEW_CONTEXT_PROP_PICKLIST.
ENDMETHOD.


method GET_V_ZZAVVCODE_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = '/WATP/HCMA_WASTE_AVVCODE'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
                         ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZINVOICE_SPLIT.
  if FVH_ZZINVOICE_SPLIT is not bound.
    FVH_ZZINVOICE_SPLIT = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_PLD_DOMAIN(
      PAR_DOMAINNAME = 'ZWR_CMGM_INVOICE_SPLIT'
      PAR_EMPTY_LINE = ABAP_TRUE ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_ZZINVOICE_SPLIT.
endmethod.


method GET_V_ZZROUTE_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = 'CRM_EWA_HROUTE_PROPOSAL'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZROUTE_DEF' F4_ATTR = 'ROUTE' )
                          ( CONTEXT_ATTR = 'STRUCT.SERVLOC' F4_ATTR = 'SERVLOC' )
                          ( CONTEXT_ATTR = 'STRUCT.ZZSERVTYPE_DEF' F4_ATTR = 'SERVICE_TYPE' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZROUTE_DEF' F4_ATTR = 'ROUTE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZSERVTYPE_DEF.
  if FVH_ZZSERVTYPE_DEF is not bound.
    FVH_ZZSERVTYPE_DEF = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_DESCRIPTOR_PLD(
      PAR_STRUCTNAME     = 'EWASSERVICETYPE'
      PAR_REQUESTTYPE    = CL_CRM_EEWA_BSP_SEARCHHELPER=>C_REQUESTTYPE_REMOTE
      PAR_KEYFIELDNAME   = 'SERVICE_TYPE'
      PAR_VALUEFIELDNAME = 'VTEXT'
      PAR_EMPTY_LINE     = ABAP_TRUE
      PAR_OBJECTTYPE     = 'EWASERVICETYPE'
                           ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_ZZSERVTYPE_DEF.
endmethod.


method GET_V_ZZWASTE_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = '/WATP/HCMA_WASTE_AVVCODE'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
                         ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZWDF_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = '/WATP/HCMA_WDPLANT_SD_CABNMARA'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
                          ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' )
                          ( CONTEXT_ATTR = 'STRUCT.VBELN' F4_ATTR = 'VBELN' )
                          ( CONTEXT_ATTR = 'STRUCT.POSNR' F4_ATTR = 'POSNR' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZWDF_DEF' F4_ATTR = 'WDPLANTNR' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


  method GET_ZZADRUNCANR.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZADRUNCANR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZADRUNCANR not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZADRVPGRPNR.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZADRVPGRPNR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZADRVPGRPNR not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZAVVCODE_DEF.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZAVVCODE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZAVVCODE_DEF not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZBILL_BLOCK.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZBILL_BLOCK' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZBILL_BLOCK not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZCONTASSIGNMENT_ID.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZCONTASSIGNMENT_ID' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZCONTASSIGNMENT_ID not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZINVOICE_SPLIT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZINVOICE_SPLIT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZINVOICE_SPLIT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZNO_PORTAL.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZNO_PORTAL' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZNO_PORTAL not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZPO_MANDAT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZPO_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZPO_MANDAT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZREQUESTER_MANDAT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZREQUESTER_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZREQUESTER_MANDAT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZSALES_PERSON.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZSALES_PERSON' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZSALES_PERSON not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZWASTE_DEF.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZWASTE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZWASTE_DEF not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZWASTE_DEF_DESCR.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZWASTE_DEF_DESCR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZWASTE_DEF_DESCR not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method SET_CONTSHARE.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'CONTSHARE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'CONTSHARE'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'CONTSHARE' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_CONT_COUNT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'CONT_COUNT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'CONT_COUNT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'CONT_COUNT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZAVVCODE_DEF.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZAVVCODE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZAVVCODE_DEF'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZAVVCODE_DEF' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZBILL_BLOCK.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZBILL_BLOCK' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZBILL_BLOCK'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZBILL_BLOCK' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZINVOICE_SPLIT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZINVOICE_SPLIT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZINVOICE_SPLIT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZINVOICE_SPLIT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZNO_PORTAL.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZNO_PORTAL' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZNO_PORTAL'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZNO_PORTAL' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZPO_MANDAT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZPO_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZPO_MANDAT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZPO_MANDAT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZREQUESTER_MANDAT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZREQUESTER_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZREQUESTER_MANDAT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZREQUESTER_MANDAT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZSALES_PERSON.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZSALES_PERSON' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZSALES_PERSON'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZSALES_PERSON' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZWASTE_DEF.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZWASTE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZWASTE_DEF'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZWASTE_DEF' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.
ENDCLASS.
class ZL_WRELOCSD_ADDSV_MAIN definition
  public
  inheriting from CL_WCF_BSP_BASE_TABLE_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_ADDSV_MAIN_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
  methods OP_TO_DETAILS
    redefinition .
  methods OP_TO_EDIT_DETAILS
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'ADDSV_MAIN.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_ADDSV_MAIN IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = 'X'.
    ev_deleteable = 'X'.
    ev_min_empty_lines = 0001.
    ev_selection_mode = 'SINGLESELECT'.
    ev_frontend_selection = 'X'.
    ev_frontend_row_creation = abap_true.
    ev_has_detail_page = 'X'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->ADDSV.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method OP_TO_DETAILS.

    super->OP_TO_DETAILS( iv_data_collection ).

    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'TO_DIFFCULT'
                            data_collection = iv_data_collection ).

  endmethod.


  method OP_TO_EDIT_DETAILS.

    super->OP_TO_EDIT_DETAILS( iv_data_collection ).

    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'TO_EDIT_DIFFCULT'
                            data_collection = iv_data_collection ).

  endmethod.


  method SET_MODELS.
*   Set the models for output
  view->set_attribute(
           name   = 'ELOCSD_NEW'                            "#EC NOTEXT
           value  = typed_context->elocsd_new ).

* Added by wizard
  view->set_attribute(
        name = 'ADDSV'                                      "#EC NOTEXT
        value  = typed_context->addsv ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ADDSV_MAIN_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ADDSV_MAIN_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_TV_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_ADDSV' ##NO_TEXT.

  methods CONSTRUCTOR .
  methods GET_V_MATNR
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .

  methods GET_OCA_T_TABLE
    redefinition .
  methods IF_BSP_MODEL~INIT
    redefinition .
*  methods ON_NEW_FOCUS
*    for event NEW_FOCUS of CL_BSP_WD_COLLECTION_WRAPPER
*    importing
*      !FOCUS_BO .
protected section.

  data FVIEW type ref to ZL_WRELOCSD_ADDSV_MAIN_IMPL .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ADDSV_MAIN_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.
    super->constructor( ).
    mv_relation_name = 'ZWRELOCSD_NEW_ADDSVS'.
  endmethod.


  method GET_OCA_T_TABLE.
    rt_actions = super->get_oca_t_table(
      EXPORTING
        iv_component = iv_component
        iv_index     = iv_index
    ).

    "remove details buttons
    delete rt_actions where id = 'DETAILS'.
  endmethod.


METHOD get_v_matnr.
  DATA:
    lprop            TYPE crm_ewa_el_props,
    lprops           TYPE crm_isu_wael_props,
    lv_zzelocsd_guid TYPE zwr_dcmgm_elocsd_guid,
    lr_context       TYPE REF TO cl_bsp_wd_context_node.

  DEFINE __add_prop.
    CLEAR lprop.
    lprop-name = &1.
    lprop-value = &2.
    APPEND lprop TO lprops.
  END-OF-DEFINITION.

  " get the main model
  lr_context ?= fview->get_model('elocsd_new').
  " read guid
  lr_context->collection_wrapper->get_current( )->get_property_as_value(
    EXPORTING
      iv_attr_name = 'ZZELOCSD_GUID'
    IMPORTING
      ev_result    = lv_zzelocsd_guid
  ).

  IF lv_zzelocsd_guid IS NOT INITIAL.
    __add_prop 'READERDOMAIN' 'ZSD'.

    data_bo_range x.
    new_bo_range x ZZELOCSD_GUID zwr_dcmgm_elocsd_guid.
    add_bo_range x ZZELOCSD_GUID lv_zzelocsd_guid.

    rv_valuehelp_descriptor = cl_crm_eewa_bsp_searchhelper=>cl_create_descriptor_pld(
                          par_objecttype     = 'MATERIAL'
                          par_requesttype    = cl_crm_eewa_bsp_searchhelper=>c_requesttype_remote
                          par_keyfieldname   = 'MATNR'
                          par_valuefieldname = 'MAKTX'
                          par_props          = lprops
                          par_ranges         = lrangexs
                          par_structname     = 'EWASMATERIAL_INFO_SHORT'
                          par_convexit_keyvalue  = 'MATN1'
                          par_empty_line = abap_true
                        ).
  ENDIF.
ENDMETHOD.


  METHOD if_bsp_model~init.
    super->if_bsp_model~init(
    EXPORTING
      id     = id
      owner  = owner
      ).
    TRY.
        fview ?= owner.
      CATCH cx_root.
        CLEAR fview.
    ENDTRY.
  ENDMETHOD.
ENDCLASS.

class ZL_WRELOCSD_ADDSV_MAIN_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ADDSV type ref to ZL_WRELOCSD_ADDSV_MAIN_CN00 read-only .
  data ELOCSD_NEW type ref to CL_BSP_WD_CONTEXT_NODE read-only .
protected section.

  methods CREATE_ADDSV .
  methods CREATE_ELOCSD_NEW .

  methods CONNECT_NODES
    redefinition .
  methods CREATE_CONTEXT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ADDSV_MAIN_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
                                                            "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED

* Added by wizard
    IF elocsd_new IS BOUND AND
       me->addsv IS BOUND.
      coll_wrapper = elocsd_new->get_collection_wrapper( ).
      SET HANDLER me->addsv->on_new_focus FOR coll_wrapper ACTIVATION iv_activate.
    ENDIF.
  endmethod.


  method CREATE_ADDSV.
    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,
      entity       TYPE REF TO cl_crm_bol_entity,    "#EC *
      entity_col   TYPE REF TO if_bol_entity_col.    "#EC *

    model = owner->create_model(

        class_name     = 'ZL_WRELOCSD_ADDSV_MAIN_CN00'

        model_id       = 'ADDSV' ). "#EC NOTEXT
    ADDSV ?= model.
    CLEAR model.

* initial setting for depandant model node
    coll_wrapper =
      ELOCSD_NEW->get_collection_wrapper( ).
    TRY.
        entity ?= coll_wrapper->get_current( ).
      CATCH cx_sy_move_cast_error.
    ENDTRY.
    IF entity IS BOUND.
      ADDSV->on_new_focus(
                   focus_bo = entity ).
    ENDIF.

  endmethod.


  method CREATE_CONTEXT_NODES.
                                                            "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).

* Added by wizard
    create_addsv( ).
  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.
ENDCLASS.


class ZL_WRELOCSD_BSPWDCOMPONE0 definition
  public
  inheriting from CL_BSP_WD_COMPONENT_CONTROLLER
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_BSPWDCOMPONE0_CTXT read-only .

  methods CONSTRUCTOR .
protected section.

  methods WD_USAGE_INITIALIZE
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants CONTROLLER type STRING value 'BSPWDComponent' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONE0 IMPLEMENTATION.


  method CONSTRUCTOR.


    super->constructor( ).
    controller_id = controller.


  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_BSPWDCOMPONE0_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.


  method WD_USAGE_INITIALIZE.


    DATA lv_node_2_bind TYPE REF TO cl_bsp_wd_context_node.

    super->wd_usage_initialize(
      iv_usage = iv_usage
         ).

    CASE iv_usage->usage_name.
      WHEN 'DUMMY'.
    ENDCASE.


  endmethod.
ENDCLASS.


class ZL_WRELOCSD_BSPWDCOMPONE0_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.
protected section.

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONE0_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_BSPWDCOMPONE0_IMPL definition
  public
  inheriting from ZL_WRELOCSD_BSPWDCOMPONE0
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONE0_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_BSPWDCOMPONE1 definition
  public
  inheriting from CL_BSP_WD_COMPONENT_CONTROLLER
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_BSPWDCOMPONE1_CTXT read-only .

  methods CONSTRUCTOR .
protected section.

  methods WD_USAGE_INITIALIZE
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants CONTROLLER type STRING value 'BSPWDComponent' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONE1 IMPLEMENTATION.


  method CONSTRUCTOR.


    super->constructor( ).
    controller_id = controller.


  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_BSPWDCOMPONE1_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.


  method WD_USAGE_INITIALIZE.


    DATA lv_node_2_bind TYPE REF TO cl_bsp_wd_context_node.

    super->wd_usage_initialize(
      iv_usage = iv_usage
         ).

    CASE iv_usage->usage_name.
      WHEN 'DUMMY'.
    ENDCASE.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_BSPWDCOMPONE1_CN00 definition
  public
  inheriting from CL_BSP_WD_CONTEXT_NODE
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_ADDSV' ##NO_TEXT.

  methods ON_NEW_FOCUS
    for event NEW_FOCUS of CL_BSP_WD_COLLECTION_WRAPPER
    importing
      !FOCUS_BO .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONE1_CN00 IMPLEMENTATION.


  method ON_NEW_FOCUS.

    DATA: lv_collection TYPE REF TO if_bol_bo_col,
          entity        TYPE REF TO cl_crm_bol_entity.

*   get collection of dependent nodes
    entity ?= focus_bo.
    TRY.
        lv_collection = entity->get_related_entities(
               iv_relation_name = 'ZWRELOCSD_NEW_ADDSVS' ).

      CATCH cx_crm_genil_model_error.
*       should never happen
        EXIT.
      CATCH cx_sy_ref_is_initial.
    ENDTRY.
    me->set_collection( lv_collection ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_BSPWDCOMPONE1_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ADDSV type ref to ZL_WRELOCSD_BSPWDCOMPONE1_CN00 read-only .
  data ELOCSD_NEW type ref to CL_BSP_WD_CONTEXT_NODE read-only .
  data DIFFCULT type ref to CL_BSP_WD_CONTEXT_NODE read-only .
protected section.

  methods CREATE_ADDSV .
  methods CREATE_ELOCSD_NEW .
  methods CREATE_DIFFCULT .

  methods CONNECT_NODES
    redefinition .
  methods CREATE_CONTEXT_NODES
    redefinition .
private section.

  data ZADDSV type ref to ZL_WRELOCSD_BSPWDCOMPONE1_CN00 .
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONE1_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
                                                            "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED

* Added by wizard
    IF elocsd_new IS BOUND AND
       me->addsv IS BOUND.
      coll_wrapper = elocsd_new->get_collection_wrapper( ).
      SET HANDLER me->addsv->on_new_focus FOR coll_wrapper ACTIVATION iv_activate.
    ENDIF.
  endmethod.


  method CREATE_ADDSV.
    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,
      entity       TYPE REF TO cl_crm_bol_entity,    "#EC *
      entity_col   TYPE REF TO if_bol_entity_col.    "#EC *

    model = owner->create_model(

        class_name     = 'ZL_WRELOCSD_BSPWDCOMPONE1_CN00'

        model_id       = 'ADDSV' ). "#EC NOTEXT
    ADDSV ?= model.
    CLEAR model.

* initial setting for depandant model node
    coll_wrapper =
      ELOCSD_NEW->get_collection_wrapper( ).
    TRY.
        entity ?= coll_wrapper->get_current( ).
      CATCH cx_sy_move_cast_error.
    ENDTRY.
    IF entity IS BOUND.
      ADDSV->on_new_focus(
                   focus_bo = entity ).
    ENDIF.

  endmethod.


  method CREATE_CONTEXT_NODES.
                                                            "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).
    create_DIFFCULT( ).

* Added by wizard
    create_addsv( ).
  endmethod.


  method CREATE_DIFFCULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'DIFFCULT' ).          "#EC NOTEXT
    DIFFCULT ?= model.
    CLEAR model.




  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.




  endmethod.
ENDCLASS.


class ZL_WRELOCSD_BSPWDCOMPONEN definition
  public
  inheriting from CL_BSP_WD_COMPONENT_CONTROLLER
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_BSPWDCOMPONEN_CTXT read-only .

  methods CONSTRUCTOR .
protected section.

  methods WD_USAGE_INITIALIZE
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants CONTROLLER type STRING value 'BSPWDComponent' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONEN IMPLEMENTATION.


  method CONSTRUCTOR.


    super->constructor( ).
    controller_id = controller.


  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_BSPWDCOMPONEN_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.


  method WD_USAGE_INITIALIZE.


    DATA lv_node_2_bind TYPE REF TO cl_bsp_wd_context_node.

    super->wd_usage_initialize(
      iv_usage = iv_usage
         ).

    CASE iv_usage->usage_name.
      WHEN 'DUMMY'.
    ENDCASE.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_BSPWDCOMPONEN_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to CL_BSP_WD_CONTEXT_NODE read-only .
  data DIFFCULTS type ref to CL_BSP_WD_CONTEXT_NODE read-only .
protected section.

  methods CREATE_ELOCSD_NEW .
  methods CREATE_DIFFCULTS .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_BSPWDCOMPONEN_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).
    create_DIFFCULTS( ).


  endmethod.


  method CREATE_DIFFCULTS.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'DIFFCULTS' ).          "#EC NOTEXT
    DIFFCULTS ?= model.
    CLEAR model.




  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.




  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULTS_DET definition
  public
  inheriting from CL_WCF_BSP_BASE_FORM_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_DIFFCULTS_DET_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'DIFFCULTS_DETAILS.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_DET IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = 'X'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->DIFFCULTS.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'DIFFCULTS'       "#EC NOTEXT
           value  = typed_context->DIFFCULTS ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_DIFFCULTS_DET_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.
class ZL_WRELOCSD_DIFFCULTS_DET_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_DIFFCULT' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_DET_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULTS_DET_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data DIFFCULTS type ref to ZL_WRELOCSD_DIFFCULTS_DET_CN00 read-only .
protected section.

  methods CREATE_DIFFCULTS .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_DET_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_DIFFCULTS( ).


  endmethod.


  method CREATE_DIFFCULTS.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_DIFFCULTS_DET_CN00'
        model_id       = 'DIFFCULTS' ).          "#EC NOTEXT
    DIFFCULTS ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'DIFFCULTS'
           iv_node_2_bind = DIFFCULTS ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULTS_MAI definition
  public
  inheriting from CL_WCF_BSP_BASE_TABLE_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_DIFFCULTS_MAI_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods OP_TO_DETAILS
    redefinition .
  methods OP_TO_EDIT_DETAILS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'DIFFCULTS_MAIN.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_MAI IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = 'X'.
    ev_deleteable = 'X'.
    ev_min_empty_lines = 0001.
    ev_selection_mode = 'SINGLESELECT'.
    ev_frontend_selection = 'X'.
    ev_frontend_row_creation = abap_true.
    ev_has_detail_page = 'X'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->DIFFCULTS.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method OP_TO_DETAILS.

    super->OP_TO_DETAILS( iv_data_collection ).

    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'TO_DIFFCULTS'
                            data_collection = iv_data_collection ).

  endmethod.


  method OP_TO_EDIT_DETAILS.

    super->OP_TO_EDIT_DETAILS( iv_data_collection ).

    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'TO_EDIT_DIFFCULTS'
                            data_collection = iv_data_collection ).

  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'ELOCSD_NEW'       "#EC NOTEXT
           value  = typed_context->ELOCSD_NEW ).
  view->set_attribute(
           name   = 'DIFFCULTS'       "#EC NOTEXT
           value  = typed_context->DIFFCULTS ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_DIFFCULTS_MAI_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.
class ZL_WRELOCSD_DIFFCULTS_MAI_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_TV_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_DIFFCULT' ##NO_TEXT.

  methods CONSTRUCTOR .
  methods GET_V_DIFFCULT
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_P_DIFFCULT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_THTMLB_OCA
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_M_THTMLB_OCA
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_P_THTMLB_OCA
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
protected section.

  data FVALUEHELP type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_MAI_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).
    mv_relation_name = 'ZWRELOCSD_NEW_DIFFCULTS'.

  endmethod.


  method GET_M_THTMLB_OCA.

    DATA: attr    TYPE crm_thtmlb_one_click_action.
    DATA: dref    TYPE REF TO data.
    GET REFERENCE OF attr INTO dref.

    metadata ?= if_bsp_model_binding~get_attribute_metadata(
         attribute_ref  = dref
         attribute_path = attribute_path
         name           = 'THTMLB_OCA'                      "#EC NOTEXT
*      COMPONENT      =
         no_getter      = 1 ).

    DATA ls_dfies TYPE dfies.
    DATA lo_metadata TYPE ref to cl_bsp_metadata_simple.

    ls_dfies-inttype = metadata->get_abap_type( ).
    ls_dfies-outputlen =  metadata->get_display_length( ).
    ls_dfies-scrtext_m = metadata->get_label( ).
    ls_dfies-leng = metadata->get_length( ).

    IF ls_dfies-scrtext_m IS INITIAL.
      ls_dfies-scrtext_m = cl_wd_utilities=>get_otr_text_by_alias( 'AXT_RUNTIME/ACTIONS' ).
    ENDIF.

    CREATE OBJECT lo_metadata
      EXPORTING
        info = ls_dfies.

    metadata ?= lo_metadata.


  endmethod.


METHOD get_p_diffcult.
  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_picklist.
  ENDCASE.
ENDMETHOD.


  method GET_P_THTMLB_OCA.

    CASE iv_property.
*field type: one click actions
      WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
        rv_value = cl_bsp_dlc_view_descriptor=>field_type_oca.
*onClick
      WHEN if_bsp_wd_model_setter_getter=>fp_onclick.
        rv_value = 'OCA'.                      "#ec notext

    ENDCASE.

  endmethod.


  method GET_THTMLB_OCA.


  endmethod.


METHOD get_v_diffcult.
  IF fvaluehelp IS NOT INITIAL.
    rv_valuehelp_descriptor = fvaluehelp.
  ELSE.
    rv_valuehelp_descriptor = cl_crm_eewa_bsp_searchhelper=>cl_create_descriptor_pld(
                              par_requesttype    = cl_crm_eewa_bsp_searchhelper=>c_requesttype_remote
                              par_keyfieldname   = 'DIFFCULT'
                              par_valuefieldname = 'VTEXT'
                              par_objecttype     = 'EWADIFFCULT'
                              par_empty_line     = abap_true
                            ).
    fvaluehelp = rv_valuehelp_descriptor.
  ENDIF.
ENDMETHOD.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULTS_MAI_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to CL_BSP_WD_CONTEXT_NODE read-only .
  data DIFFCULTS type ref to ZL_WRELOCSD_DIFFCULTS_MAI_CN00 read-only .
protected section.

  methods CREATE_ELOCSD_NEW .
  methods CREATE_DIFFCULTS .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_MAI_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED

    coll_wrapper = me->ELOCSD_NEW->get_collection_wrapper( ).
    SET HANDLER me->DIFFCULTS->on_new_focus
                FOR coll_wrapper ACTIVATION iv_activate.



  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).
    create_DIFFCULTS( ).


  endmethod.


  method CREATE_DIFFCULTS.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_DIFFCULTS_MAI_CN00'
        model_id       = 'DIFFCULTS' ).          "#EC NOTEXT
    DIFFCULTS ?= model.
    CLEAR model.


    " initial setting for dependant model node
    coll_wrapper =
      ELOCSD_NEW->get_collection_wrapper( ).
    TRY.
        entity ?= coll_wrapper->get_current( ).
      CATCH cx_sy_move_cast_error.  "#EC NO_HANDLER
    ENDTRY.
    IF entity IS BOUND.
      DIFFCULTS->on_new_focus(
                   focus_bo = entity ).
    ENDIF.


  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULTS_OVP definition
  public
  inheriting from CL_WCF_BSP_BASE_OVP
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_DIFFCULTS_OVP_CTXT read-only .

  methods OP_TO_ELOCSD_NEW
    importing
      !IV_DATA_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods DO_DETERMINE_CONFIG_KEYS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_OVP IMPLEMENTATION.


  method DO_DETERMINE_CONFIG_KEYS.

  ev_object_type = 'ZWRELOCSD_NEW'.

  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    DATA: ls_back_plug TYPE axts_ui_back_plugs.

    ev_editable   = 'X'.
    ev_deleteable = 'X'.
    ev_main       = ''.

    ls_back_plug-object_name = 'ZWRELOCSD_NEW'.
    ls_back_plug-outbound_plug = 'OP_TO_ELOCSD_NEW'.
    APPEND ls_back_plug TO et_back_plugs.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->DIFFCULTS.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method OP_TO_ELOCSD_NEW.


    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'BACK_FROM_DIFFCULTS'
                            data_collection = iv_data_collection ).

  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_DIFFCULTS_OVP_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.
class ZL_WRELOCSD_DIFFCULTS_OVP_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_DIFFCULT' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_OVP_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.



class ZL_WRELOCSD_DIFFCULTS_OVP_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data DIFFCULTS type ref to ZL_WRELOCSD_DIFFCULTS_OVP_CN00 read-only .

  methods IF_BSP_WD_HISTORY_STATE~STATE_STORE
    redefinition .
  methods IF_BSP_WD_HISTORY_STATE~STATE_RESTORE
    redefinition .
protected section.

  methods CREATE_DIFFCULTS .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_OVP_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_DIFFCULTS( ).


  endmethod.


  method CREATE_DIFFCULTS.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_DIFFCULTS_OVP_CN00'
        model_id       = 'DIFFCULTS' ).          "#EC NOTEXT
    DIFFCULTS ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'DIFFCULTS'
           iv_node_2_bind = DIFFCULTS ).



  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_RESTORE.

    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    state_container->get_item( EXPORTING im_name  = 'DIFFCULTS'
                               IMPORTING ex_value = lo_ent
                               EXCEPTIONS OTHERS = 0 ).
    CHECK lo_ent IS BOUND.
    me->diffcults->collection_wrapper->clear( ).
    me->diffcults->collection_wrapper->add( iv_entity = lo_ent
                                                            iv_set_focus = abap_true ).


  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_STORE.


    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    lo_ent ?= me->diffcults->collection_wrapper->get_current( ).
    IF lo_ent IS BOUND.
      state_container->add_item( im_name  = 'DIFFCULTS'
                                 im_value =  lo_ent ).
    ENDIF.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULTS_OVP_IMPL definition
  public
  inheriting from ZL_WRELOCSD_DIFFCULTS_OVP
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULTS_OVP_IMPL IMPLEMENTATION.
ENDCLASS.


class ZL_WRELOCSD_DIFFCULT_MAIN definition
  public
  inheriting from CL_WCF_BSP_BASE_TABLE_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_DIFFCULT_MAIN_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods OP_TO_DETAILS
    redefinition .
  methods OP_TO_EDIT_DETAILS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'DIFFCULT_MAIN.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_MAIN IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = 'X'.
    ev_deleteable = 'X'.
    ev_min_empty_lines = 0001.
    ev_selection_mode = 'SINGLESELECT'.
    ev_frontend_selection = 'X'.
    ev_frontend_row_creation = abap_true.
    ev_has_detail_page = 'X'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->DIFFCULT.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method OP_TO_DETAILS.

    super->OP_TO_DETAILS( iv_data_collection ).

    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'TO_DIFFCULT'
                            data_collection = iv_data_collection ).

  endmethod.


  method OP_TO_EDIT_DETAILS.

    super->OP_TO_EDIT_DETAILS( iv_data_collection ).

    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'TO_EDIT_DIFFCULT'
                            data_collection = iv_data_collection ).

  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'ELOCSD_NEW'       "#EC NOTEXT
           value  = typed_context->ELOCSD_NEW ).
  view->set_attribute(
           name   = 'DIFFCULT'       "#EC NOTEXT
           value  = typed_context->DIFFCULT ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_DIFFCULT_MAIN_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_MAIN_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_TV_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_DIFFCULT' ##NO_TEXT.

  methods GET_DIFFCTXT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_I_DIFFCTXT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_M_DIFFCTXT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods SET_DIFFCTXT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods CONSTRUCTOR .
  methods GET_V_DIFFCULT
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_P_DIFFCULT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_THTMLB_OCA
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_M_THTMLB_OCA
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_P_THTMLB_OCA
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .

  methods GET_OCA_T_TABLE
    redefinition .
protected section.

  data FVALUEHELP type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_MAIN_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).
    mv_relation_name = 'ZWRELOCSD_NEW_DIFFCULTS'.

  endmethod.


  method GET_DIFFCTXT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW_DIFFCULT not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'DIFFCTXT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW_DIFFCULT/DIFFCTXT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_I_DIFFCTXT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'DIFFCTXT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  METHOD GET_M_DIFFCTXT.

    DATA: ATTR    TYPE TEXT50.

    DATA: DREF    TYPE REF TO DATA.

    GET REFERENCE OF ATTR INTO DREF.

    METADATA ?= IF_BSP_MODEL_BINDING~GET_ATTRIBUTE_METADATA(
         ATTRIBUTE_REF  = DREF
         ATTRIBUTE_PATH = ATTRIBUTE_PATH
         NAME           = 'DIFFCTXT'                        "#EC NOTEXT
*      COMPONENT      =
         NO_GETTER      = 1 ).


  ENDMETHOD.


  method GET_M_THTMLB_OCA.

    DATA: attr    TYPE crm_thtmlb_one_click_action.
    DATA: dref    TYPE REF TO data.
    GET REFERENCE OF attr INTO dref.

    metadata ?= if_bsp_model_binding~get_attribute_metadata(
         attribute_ref  = dref
         attribute_path = attribute_path
         name           = 'THTMLB_OCA'                      "#EC NOTEXT
*      COMPONENT      =
         no_getter      = 1 ).

    DATA ls_dfies TYPE dfies.
    DATA lo_metadata TYPE ref to cl_bsp_metadata_simple.

    ls_dfies-inttype = metadata->get_abap_type( ).
    ls_dfies-outputlen =  metadata->get_display_length( ).
    ls_dfies-scrtext_m = metadata->get_label( ).
    ls_dfies-leng = metadata->get_length( ).

    IF ls_dfies-scrtext_m IS INITIAL.
      ls_dfies-scrtext_m = cl_wd_utilities=>get_otr_text_by_alias( 'AXT_RUNTIME/ACTIONS' ).
    ENDIF.

    CREATE OBJECT lo_metadata
      EXPORTING
        info = ls_dfies.

    metadata ?= lo_metadata.


  endmethod.


  method GET_OCA_T_TABLE.
    rt_actions = super->get_oca_t_table(
      EXPORTING
        iv_component = iv_component
        iv_index     = iv_index
    ).

    "remove details buttons
    delete rt_actions where id = 'DETAILS'.
  endmethod.


METHOD GET_P_DIFFCULT.
 case IV_PROPERTY.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_FIELDTYPE.
      RV_VALUE = CL_BSP_DLC_VIEW_DESCRIPTOR=>FIELD_TYPE_PICKLIST.
  endcase.
ENDMETHOD.


  method GET_P_THTMLB_OCA.

    CASE iv_property.
*field type: one click actions
      WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
        rv_value = cl_bsp_dlc_view_descriptor=>field_type_oca.
*onClick
      WHEN if_bsp_wd_model_setter_getter=>fp_onclick.
        rv_value = 'OCA'.                      "#ec notext

    ENDCASE.

  endmethod.


  method GET_THTMLB_OCA.


  endmethod.


METHOD get_v_diffcult.
  IF fvaluehelp IS NOT INITIAL.
    rv_valuehelp_descriptor = fvaluehelp.
  ELSE.
    rv_valuehelp_descriptor = cl_crm_eewa_bsp_searchhelper=>cl_create_descriptor_pld(
                              par_requesttype    = cl_crm_eewa_bsp_searchhelper=>c_requesttype_remote
                              par_keyfieldname   = 'DIFFCULT'
                              par_valuefieldname = 'VTEXT'
                              par_objecttype     = 'EWADIFFCULT'
                              par_empty_line     = abap_true
                            ).
    fvaluehelp = rv_valuehelp_descriptor.
  ENDIF.
ENDMETHOD.


  method SET_DIFFCTXT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'DIFFCTXT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'DIFFCTXT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'DIFFCTXT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_MAIN_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to CL_BSP_WD_CONTEXT_NODE read-only .
  data DIFFCULT type ref to ZL_WRELOCSD_DIFFCULT_MAIN_CN00 read-only .
protected section.

  methods CREATE_ELOCSD_NEW .
  methods CREATE_DIFFCULT .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_MAIN_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED

    coll_wrapper = me->ELOCSD_NEW->get_collection_wrapper( ).
    SET HANDLER me->DIFFCULT->on_new_focus
                FOR coll_wrapper ACTIVATION iv_activate.



  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).
    create_DIFFCULT( ).


  endmethod.


  method CREATE_DIFFCULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_DIFFCULT_MAIN_CN00'
        model_id       = 'DIFFCULT' ).          "#EC NOTEXT
    DIFFCULT ?= model.
    CLEAR model.


    " initial setting for dependant model node
    coll_wrapper =
      ELOCSD_NEW->get_collection_wrapper( ).
    TRY.
        entity ?= coll_wrapper->get_current( ).
      CATCH cx_sy_move_cast_error.  "#EC NO_HANDLER
    ENDTRY.
    IF entity IS BOUND.
      DIFFCULT->on_new_focus(
                   focus_bo = entity ).
    ENDIF.


  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_MAIN_IMPL definition
  public
  inheriting from ZL_WRELOCSD_DIFFCULT_MAIN
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_MAIN_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_OVP definition
  public
  inheriting from CL_WCF_BSP_BASE_OVP
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_DIFFCULT_OVP_CTXT read-only .

  methods OP_TO_ELOCSD_NEW
    importing
      !IV_DATA_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods DO_DETERMINE_CONFIG_KEYS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_OVP IMPLEMENTATION.


  method DO_DETERMINE_CONFIG_KEYS.

  ev_object_type = 'ZWRELOCSD_DF'.

  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    DATA: ls_back_plug TYPE axts_ui_back_plugs.

    ev_editable   = 'X'.
    ev_deleteable = 'X'.
    ev_main       = ''.

    ls_back_plug-object_name = 'ZWRELOCSD_NEW'.
    ls_back_plug-outbound_plug = 'OP_TO_ELOCSD_NEW'.
    APPEND ls_back_plug TO et_back_plugs.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->DIFFCULT.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method OP_TO_ELOCSD_NEW.


    view_manager->navigate( source_rep_view = rep_view
                            outbound_plug   = 'BACK_FROM_DIFFCULT'
                            data_collection = iv_data_collection ).

  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_DIFFCULT_OVP_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_OVP_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW_DIFFCULT' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_OVP_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_OVP_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data DIFFCULT type ref to ZL_WRELOCSD_DIFFCULT_OVP_CN00 read-only .

  methods IF_BSP_WD_HISTORY_STATE~STATE_STORE
    redefinition .
  methods IF_BSP_WD_HISTORY_STATE~STATE_RESTORE
    redefinition .
protected section.

  methods CREATE_DIFFCULT .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_OVP_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_DIFFCULT( ).


  endmethod.


  method CREATE_DIFFCULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_DIFFCULT_OVP_CN00'
        model_id       = 'DIFFCULT' ).          "#EC NOTEXT
    DIFFCULT ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'DIFFCULT'
           iv_node_2_bind = DIFFCULT ).



  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_RESTORE.

    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    state_container->get_item( EXPORTING im_name  = 'DIFFCULT'
                               IMPORTING ex_value = lo_ent
                               EXCEPTIONS OTHERS = 0 ).
    CHECK lo_ent IS BOUND.
    me->diffcult->collection_wrapper->clear( ).
    me->diffcult->collection_wrapper->add( iv_entity = lo_ent
                                                            iv_set_focus = abap_true ).


  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_STORE.


    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    lo_ent ?= me->diffcult->collection_wrapper->get_current( ).
    IF lo_ent IS BOUND.
      state_container->add_item( im_name  = 'DIFFCULT'
                                 im_value =  lo_ent ).
    ENDIF.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_DIFFCULT_OVP_IMPL definition
  public
  inheriting from ZL_WRELOCSD_DIFFCULT_OVP
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_DIFFCULT_OVP_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_D0 definition
  public
  inheriting from CL_WCF_BSP_BASE_FORM_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_ELOCSD_NEW_D0_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'ELOCSD_NEW_DETAILS.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_D0 IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = ''.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'ELOCSD_NEW'       "#EC NOTEXT
           value  = typed_context->ELOCSD_NEW ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ELOCSD_NEW_D0_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.


class ZL_WRELOCSD_ELOCSD_NEW_D0_CN00 definition
  public
  inheriting from CL_BSP_WD_CONTEXT_NODE
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW' ##NO_TEXT.

  methods GET_I_ZZ_AETIMESTAMP
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_ERDAT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_ERLANGU
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_ERNAM
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_ERTIME
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_ERTIMESTAMP
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_M_ZZDATE_REPLACE
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZTIDNR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZTIDNR_REPLACE
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_AEDAT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_AENAM
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_AETIME
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_AETIMESTAMP
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_ERDAT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_ERLANGU
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_ERNAM
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_ERTIME
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_ERTIMESTAMP
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_ZZDATE_REPLACE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZTIDNR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZTIDNR_REPLACE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_AEDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_AENAM
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_AETIME
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_AETIMESTAMP
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_ERDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_ERLANGU
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_ERNAM
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_ERTIME
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_ERTIMESTAMP
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods SET_ZZDATE_REPLACE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZTIDNR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZTIDNR_REPLACE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_AEDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_AENAM
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_AETIME
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_AETIMESTAMP
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_ERDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_ERLANGU
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_ERNAM
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_ERTIME
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_ERTIMESTAMP
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods GET_I_ZZDATE_REPLACE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZTIDNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZTIDNR_REPLACE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_AEDAT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_AETIME
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_AENAM
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_CONTSHARE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_H_ZZTIDNR_REPLACE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_HIDDEN) type STRING .
  methods GET_H_ZZDATE_REPLACE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_HIDDEN) type STRING .
  methods GET_CONT_COUNT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_I_AB
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_P_ZZTIDNR_REPLACE
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_I_BIS
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_CONTSHARE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_CONT_COUNT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_EQUNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_IBASE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ILOAN
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_KUNNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_LOGIKNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_MATNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_OBKNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_OBZAE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_POSNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_SERVFREQNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_SERVLOC
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_TIMFR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_TIMTO
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_VBELN
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZADRUNCANR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZADRVPGRPNR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZAVVCODE_DEF
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZBILL_BLOCK
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZCONTASSIGNMENT_ID
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZINVOICE_SPLIT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZLATITUDE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZLONGITUDE
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZNO_PORTAL
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZPO_MANDAT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZREQUESTER_MANDAT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZSALES_PERSON
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZWASTE_DEF
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZWASTE_DEF_DESCR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_M_CONTSHARE
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_CONT_COUNT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZADRUNCANR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZADRVPGRPNR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZAVVCODE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZBILL_BLOCK
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZCONTASSIGNMENT_ID
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZINVOICE_SPLIT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZLATITUDE
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZLONGITUDE
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZNO_PORTAL
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZPO_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZREQUESTER_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZSALES_PERSON
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZWASTE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZWASTE_DEF_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_P_CONTSHARE
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_CONT_COUNT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZAVVCODE_DEF
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZBILL_BLOCK
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZINVOICE_SPLIT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZNO_PORTAL
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZPO_MANDAT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZREQUESTER_MANDAT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZSERVTYPE_DEF
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_V_ZZTIDNR_REPLACE
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZAVVCODE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZBILL_BLOCK
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZINVOICE_SPLIT
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZROUTE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZSERVTYPE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZWASTE_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZWDF_DEF
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_ZZADRUNCANR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZADRVPGRPNR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZAVVCODE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZBILL_BLOCK
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZCONTASSIGNMENT_ID
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZINVOICE_SPLIT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZLATITUDE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZLONGITUDE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZNO_PORTAL
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZPO_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZREQUESTER_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZSALES_PERSON
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZWASTE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZWASTE_DEF_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods SET_CONTSHARE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_CONT_COUNT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZAVVCODE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZBILL_BLOCK
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZINVOICE_SPLIT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZLATITUDE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZLONGITUDE
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZNO_PORTAL
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZPO_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZREQUESTER_MANDAT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZSALES_PERSON
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZWASTE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
protected section.

  data FVH_ZZBILL_BLOCK type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  data FVH_ZZINVOICE_SPLIT type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  data FVH_ZZSERVTYPE_DEF type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_D0_CN00 IMPLEMENTATION.


  method GET_CONTSHARE.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'CONTSHARE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/CONTSHARE not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_CONT_COUNT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'CONT_COUNT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/CONT_COUNT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  METHOD GET_H_ZZDATE_REPLACE.
    DATA: CURRENT        TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS,
          ENTITY         TYPE REF TO CL_CRM_BOL_ENTITY,
          LV_HEADER_GUID TYPE CRMT_OBJECT_GUID.
    DATA: LV_PROFILE_NAME TYPE TIMECONT,
          LT_PROFILE      TYPE CRMT_DATES_PRF,
          LS_PROFILE      LIKE LINE OF LT_PROFILE.
    INCLUDE CRM_OBJECT_KINDS_CON.

    IF ITERATOR IS BOUND.
      CURRENT = ITERATOR->GET_CURRENT( ).
    ELSE.
      CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
    ENDIF.

    TRY.
        ENTITY ?= CURRENT.
      CATCH CX_CRM_CIC_PARAMETER_ERROR
        CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
        CX_CRM_GENIL_MODEL_ERROR.
        RETURN.
    ENDTRY.

    if ENTITY->IS_LOCKED( ) = ABAP_TRUE.
      RV_HIDDEN = ABAP_FALSE.
    else.
      RV_HIDDEN = ABAP_TRUE.
    endif.
  ENDMETHOD.


  METHOD GET_H_ZZTIDNR_REPLACE.
    DATA: CURRENT        TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS,
          ENTITY         TYPE REF TO CL_CRM_BOL_ENTITY.

    IF ITERATOR IS BOUND.
      CURRENT = ITERATOR->GET_CURRENT( ).
    ELSE.
      CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
    ENDIF.

    TRY.
        ENTITY ?= CURRENT.

      CATCH CX_CRM_CIC_PARAMETER_ERROR
        CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
        CX_CRM_GENIL_MODEL_ERROR.
        RETURN.
    ENDTRY.
    if ENTITY->IS_LOCKED( ) = ABAP_TRUE.
      RV_HIDDEN = ABAP_FALSE.
    else.
      RV_HIDDEN = ABAP_TRUE.
    endif.
  ENDMETHOD.


method GET_I_AB.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_BIS.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_CONTSHARE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_CONT_COUNT.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_EQUNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_IBASE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ILOAN.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_KUNNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_LOGIKNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_MATNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_OBKNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_OBZAE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_POSNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_SERVFREQNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_SERVLOC.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_TIMFR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_TIMTO.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_VBELN.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ZZADRUNCANR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ZZADRVPGRPNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


method GET_I_ZZAVVCODE_DEF.
  data: CURRENT type ref to IF_BOL_BO_PROPERTY_ACCESS.

  RV_DISABLED = 'TRUE'.
  if ITERATOR is bound.
    CURRENT = ITERATOR->GET_CURRENT( ).
  else.
    CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
  endif.

  try.

      if CURRENT->IS_PROPERTY_READONLY(
                    'ZZAVVCODE_DEF' ) = ABAP_FALSE.         "#EC NOTEXT
        RV_DISABLED = 'FALSE'.
      endif.

    catch CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
          CX_CRM_GENIL_MODEL_ERROR.
      return.
  endtry.



endmethod.


  method GET_I_ZZBILL_BLOCK.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZBILL_BLOCK' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


method GET_I_ZZCONTASSIGNMENT_ID.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


  method GET_I_ZZDATE_REPLACE.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZDATE_REPLACE' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZINVOICE_SPLIT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZINVOICE_SPLIT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZLATITUDE.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZLATITUDE' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZLONGITUDE.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZLONGITUDE' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZNO_PORTAL.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZNO_PORTAL' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZPO_MANDAT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZPO_MANDAT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZREQUESTER_MANDAT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZREQUESTER_MANDAT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZSALES_PERSON.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZSALES_PERSON' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZTIDNR.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZTIDNR' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZTIDNR_REPLACE.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZTIDNR_REPLACE' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZWASTE_DEF.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZWASTE_DEF' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


method GET_I_ZZWASTE_DEF_DESCR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


  method GET_I_ZZ_AEDAT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_AEDAT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_AENAM.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_AENAM' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_AETIME.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_AETIME' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_AETIMESTAMP.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_AETIMESTAMP' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_ERDAT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_ERDAT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_ERLANGU.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_ERLANGU' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_ERNAM.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_ERNAM' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_ERTIME.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_ERTIME' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZ_ERTIMESTAMP.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZ_ERTIMESTAMP' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_M_CONTSHARE.

  DATA: attr    TYPE string.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'CONTSHARE'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_CONT_COUNT.

  DATA: attr    TYPE NUMC4.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'CONT_COUNT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZADRUNCANR.

  DATA: attr    TYPE /WATP/DADRUNCANR.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZADRUNCANR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZADRVPGRPNR.

  DATA: attr    TYPE /WATP/DADRVPGRPNR.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZADRVPGRPNR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZAVVCODE_DEF.

  DATA: attr    TYPE /WATP/DAVVCODE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZAVVCODE_DEF'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZBILL_BLOCK.

  DATA: attr    TYPE ZWR_DCMGM_BILL_BLOCK.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZBILL_BLOCK'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZCONTASSIGNMENT_ID.

  DATA: attr    TYPE ZWR_DCMGM_CONTASSIGNMENT_ID.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZCONTASSIGNMENT_ID'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZDATE_REPLACE.

  DATA: attr    TYPE ZWR_DCMGM_DATE_REPLACE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZDATE_REPLACE'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZINVOICE_SPLIT.

  DATA: attr    TYPE ZWR_DCMGM_INVOICE_SPLIT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZINVOICE_SPLIT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZLATITUDE.

  DATA: attr    TYPE /PLCE/LATITUDE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZLATITUDE'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZLONGITUDE.

  DATA: attr    TYPE /PLCE/LONGITUDE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZLONGITUDE'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZNO_PORTAL.

  DATA: attr    TYPE ZWR_DCMGM_NO_PORTAL.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZNO_PORTAL'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZPO_MANDAT.

  DATA: attr    TYPE ZWR_DCMGM_PO_MANDAT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZPO_MANDAT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZREQUESTER_MANDAT.

  DATA: attr    TYPE ZWR_DCMGM_REQUESTER_MANDAT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZREQUESTER_MANDAT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZSALES_PERSON.

  DATA: attr    TYPE ZWR_DCMGM_SALES_PERSON.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZSALES_PERSON'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZTIDNR.

  DATA: attr    TYPE TIDNR.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZTIDNR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZTIDNR_REPLACE.

  DATA: attr    TYPE ZWR_DCMGM_TIDNR_REPLACE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZTIDNR_REPLACE'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZWASTE_DEF.

  DATA: attr    TYPE WASTE_TYPE.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZWASTE_DEF'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZWASTE_DEF_DESCR.

  DATA: attr    TYPE MAKTX.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZWASTE_DEF_DESCR'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_AEDAT.

  DATA: attr    TYPE AEDAT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_AEDAT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_AENAM.

  DATA: attr    TYPE AENAM.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_AENAM'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_AETIME.

  DATA: attr    TYPE EWADAETIME.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_AETIME'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_AETIMESTAMP.

  DATA: attr    TYPE TIMESTAMPL.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_AETIMESTAMP'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_ERDAT.

  DATA: attr    TYPE ERDAT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_ERDAT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_ERLANGU.

  DATA: attr    TYPE EWADADMCREALANGU.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_ERLANGU'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_ERNAM.

  DATA: attr    TYPE ERNAM.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_ERNAM'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_ERTIME.

  DATA: attr    TYPE EWADERTIME.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_ERTIME'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZ_ERTIMESTAMP.

  DATA: attr    TYPE TIMESTAMPL.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZ_ERTIMESTAMP'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


method GET_P_CONTSHARE.
  case IV_PROPERTY .
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_FIELDTYPE.
      RV_VALUE = CL_BSP_DLC_VIEW_DESCRIPTOR=>FIELD_TYPE_INPUT.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_TEXT_DIRECTION.
      RV_VALUE = CL_THTMLB_COMMON_CONSTANT=>GC_TEXTDIRECTION_LTR.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_CUSTOM_CSS.
      RV_VALUE = 'th-tx-l' ##NO_TEXT.
  endcase.
endmethod.


method GET_P_CONT_COUNT.
  case IV_PROPERTY .
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_FIELDTYPE.
      RV_VALUE = CL_BSP_DLC_VIEW_DESCRIPTOR=>FIELD_TYPE_INPUT.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_TEXT_DIRECTION.
      RV_VALUE = CL_THTMLB_COMMON_CONSTANT=>GC_TEXTDIRECTION_LTR.
    when IF_BSP_WD_MODEL_SETTER_GETTER=>FP_CUSTOM_CSS.
      RV_VALUE = 'th-tx-l' ##NO_TEXT.
  endcase.
endmethod.


METHOD GET_P_ZZAVVCODE_DEF.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      "rv_value = cl_bsp_dlc_view_descriptor=>field_type_input.

    "WHEN IF_BSP_WD_MODEL_SETTER_GETTER=>...
      "...
  ENDCASE.

ENDMETHOD.


method GET_P_ZZBILL_BLOCK.
  VIEW_CONTEXT_PROP_PICKLIST.
endmethod.


method GET_P_ZZINVOICE_SPLIT.
  VIEW_CONTEXT_PROP_PICKLIST.
endmethod.


method GET_P_ZZNO_PORTAL.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


method GET_P_ZZPO_MANDAT.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


method GET_P_ZZREQUESTER_MANDAT.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


METHOD GET_P_ZZSERVTYPE_DEF.
  VIEW_CONTEXT_PROP_PICKLIST.
ENDMETHOD.


METHOD GET_P_ZZTIDNR_REPLACE.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      "rv_value = cl_bsp_dlc_view_descriptor=>field_type_input.

    "WHEN IF_BSP_WD_MODEL_SETTER_GETTER=>...
      "...
  ENDCASE.

ENDMETHOD.


method GET_V_ZZAVVCODE_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = '/WATP/HCMA_WASTE_AVVCODE'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
                         ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZBILL_BLOCK.
  if FVH_ZZBILL_BLOCK is not bound.
    data(LT_SELECTION_TABLE) = value BSP_WD_DROPDOWN_TABLE( ).
    select A~FAKSP as KEY
          ,A~VTEXT as VALUE
      from TVFST as A
     where A~SPRAS = @SY-LANGU
      into corresponding fields of table @LT_SELECTION_TABLE.
    insert value #( ) into LT_SELECTION_TABLE index 1.
    FVH_ZZBILL_BLOCK = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_DESCRIPTOR_PLD_TABLE( PAR_SELECTION_TABLE = LT_SELECTION_TABLE ).
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_ZZBILL_BLOCK.
endmethod.


method GET_V_ZZINVOICE_SPLIT.
  if FVH_ZZINVOICE_SPLIT is not bound.
    FVH_ZZINVOICE_SPLIT = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_PLD_DOMAIN(
      PAR_DOMAINNAME = 'ZWR_CMGM_INVOICE_SPLIT'
      PAR_EMPTY_LINE = ABAP_TRUE ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_ZZINVOICE_SPLIT.
endmethod.


method GET_V_ZZROUTE_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = 'CRM_EWA_HROUTE_PROPOSAL'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZROUTE_DEF' F4_ATTR = 'ROUTE' )
                          ( CONTEXT_ATTR = 'STRUCT.SERVLOC' F4_ATTR = 'SERVLOC' )
                          ( CONTEXT_ATTR = 'STRUCT.ZZSERVTYPE_DEF' F4_ATTR = 'SERVICE_TYPE' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZROUTE_DEF' F4_ATTR = 'ROUTE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZSERVTYPE_DEF.
  if FVH_ZZSERVTYPE_DEF is not bound.
    FVH_ZZSERVTYPE_DEF = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_DESCRIPTOR_PLD(
      PAR_STRUCTNAME     = 'EWASSERVICETYPE'
      PAR_REQUESTTYPE    = CL_CRM_EEWA_BSP_SEARCHHELPER=>C_REQUESTTYPE_REMOTE
      PAR_KEYFIELDNAME   = 'SERVICE_TYPE'
      PAR_VALUEFIELDNAME = 'VTEXT'
      PAR_EMPTY_LINE     = ABAP_TRUE
      PAR_OBJECTTYPE     = 'EWASERVICETYPE'
                           ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_ZZSERVTYPE_DEF.
endmethod.


method GET_V_ZZTIDNR_REPLACE.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = 'ZWR_EEWA_EQUZ_TIDNR'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZTIDNR_REPLACE' F4_ATTR = 'TIDNR' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZTIDNR_REPLACE' F4_ATTR = 'TIDNR' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZWASTE_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = '/WATP/HCMA_WASTE_AVVCODE'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
                         ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


method GET_V_ZZWDF_DEF.
  RV_VALUEHELP_DESCRIPTOR = new CL_BSP_WD_VALUEHELP_F4DESCR(
    IV_HELP_ID        = '/WATP/HCMA_WDPLANT_SD_CABNMARA'
    IV_HELP_ID_KIND   = IF_BSP_WD_VALUEHELP_F4DESCR=>HELP_ID_KIND_COMP
    IV_INPUT_MAPPING  = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                          ( CONTEXT_ATTR = 'STRUCT.ZZWASTE_DEF' F4_ATTR = 'MATNR' )
                          ( CONTEXT_ATTR = 'STRUCT.ZZAVVCODE_DEF' F4_ATTR = 'AVVCODE' )
                          ( CONTEXT_ATTR = 'STRUCT.VBELN' F4_ATTR = 'VBELN' )
                          ( CONTEXT_ATTR = 'STRUCT.POSNR' F4_ATTR = 'POSNR' ) )
    IV_OUTPUT_MAPPING = value IF_BSP_WD_VALUEHELP_F4DESCR=>GTYPE_PARAM_MAPPING_TAB(
                         ( CONTEXT_ATTR = 'STRUCT.ZZWDF_DEF' F4_ATTR = 'WDPLANTNR' ) )
    IV_OBJECT_REF     = ME ) ##NO_TEXT.
endmethod.


  method GET_ZZADRUNCANR.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZADRUNCANR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZADRUNCANR not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZADRVPGRPNR.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZADRVPGRPNR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZADRVPGRPNR not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZAVVCODE_DEF.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZAVVCODE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZAVVCODE_DEF not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZBILL_BLOCK.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZBILL_BLOCK' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZBILL_BLOCK not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZCONTASSIGNMENT_ID.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZCONTASSIGNMENT_ID' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZCONTASSIGNMENT_ID not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZDATE_REPLACE.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZDATE_REPLACE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZDATE_REPLACE not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZINVOICE_SPLIT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZINVOICE_SPLIT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZINVOICE_SPLIT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZLATITUDE.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZLATITUDE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZLATITUDE not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZLONGITUDE.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZLONGITUDE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZLONGITUDE not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZNO_PORTAL.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZNO_PORTAL' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZNO_PORTAL not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZPO_MANDAT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZPO_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZPO_MANDAT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZREQUESTER_MANDAT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZREQUESTER_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZREQUESTER_MANDAT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZSALES_PERSON.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZSALES_PERSON' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZSALES_PERSON not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  METHOD GET_ZZTIDNR.

    DATA: CURRENT  TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS.
    DATA: DREF     TYPE REF TO DATA.
    DATA: LV_EQUNR TYPE EQUNR.

    VALUE =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF ITERATOR IS BOUND.
      CURRENT = ITERATOR->GET_CURRENT( ).
    ELSE.
      CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
    ENDIF.


    TRY.

        TRY.

            CURRENT->GET_PROPERTY_AS_VALUE(
              EXPORTING IV_ATTR_NAME = 'EQUNR'
              IMPORTING EV_RESULT    = LV_EQUNR
            ).

          CATCH CX_CRM_CIC_PARAMETER_ERROR.
        ENDTRY.

      CATCH CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
            CX_CRM_GENIL_MODEL_ERROR.
        RETURN.
    ENDTRY.

    TRY.

        SELECT SINGLE TIDNR
          FROM EQUZ
         WHERE EQUNR = @LV_EQUNR
           AND DATBI >= @SY-DATUM
          INTO @VALUE.

      CATCH CX_BSP_CONV_ILLEGAL_REF.
        FIELD-SYMBOLS: <L_DATA> TYPE DATA.
        ASSIGN DREF->* TO <L_DATA>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        VALUE = '-CURR/QUANT REF DATA MISSING-'.
      CATCH CX_ROOT.
        VALUE = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.


  ENDMETHOD.


  method GET_ZZTIDNR_REPLACE.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZTIDNR_REPLACE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZTIDNR_REPLACE not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZWASTE_DEF.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZWASTE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZWASTE_DEF not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZWASTE_DEF_DESCR.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZWASTE_DEF_DESCR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZWASTE_DEF_DESCR not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_AEDAT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AEDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_AEDAT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_AENAM.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AENAM' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_AENAM not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_AETIME.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AETIME' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_AETIME not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_AETIMESTAMP.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AETIMESTAMP' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_AETIMESTAMP not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_ERDAT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_ERDAT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_ERLANGU.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERLANGU' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_ERLANGU not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_ERNAM.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERNAM' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_ERNAM not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_ERTIME.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERTIME' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_ERTIME not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method GET_ZZ_ERTIMESTAMP.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERTIMESTAMP' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZ_ERTIMESTAMP not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  method SET_CONTSHARE.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'CONTSHARE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'CONTSHARE'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'CONTSHARE' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_CONT_COUNT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'CONT_COUNT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'CONT_COUNT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'CONT_COUNT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZAVVCODE_DEF.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZAVVCODE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZAVVCODE_DEF'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZAVVCODE_DEF' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZBILL_BLOCK.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZBILL_BLOCK' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZBILL_BLOCK'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZBILL_BLOCK' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZDATE_REPLACE.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZDATE_REPLACE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZDATE_REPLACE'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZDATE_REPLACE' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZINVOICE_SPLIT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZINVOICE_SPLIT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZINVOICE_SPLIT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZINVOICE_SPLIT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZLATITUDE.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZLATITUDE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZLATITUDE'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZLATITUDE' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZLONGITUDE.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZLONGITUDE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZLONGITUDE'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZLONGITUDE' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZNO_PORTAL.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZNO_PORTAL' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZNO_PORTAL'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZNO_PORTAL' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZPO_MANDAT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZPO_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZPO_MANDAT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZPO_MANDAT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZREQUESTER_MANDAT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZREQUESTER_MANDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZREQUESTER_MANDAT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZREQUESTER_MANDAT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZSALES_PERSON.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZSALES_PERSON' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZSALES_PERSON'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZSALES_PERSON' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZTIDNR.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZTIDNR' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZTIDNR'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZTIDNR' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZTIDNR_REPLACE.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZTIDNR_REPLACE' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZTIDNR_REPLACE'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZTIDNR_REPLACE' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZWASTE_DEF.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZWASTE_DEF' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZWASTE_DEF'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZWASTE_DEF' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_AEDAT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AEDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_AEDAT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_AEDAT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_AENAM.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AENAM' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_AENAM'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_AENAM' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_AETIME.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AETIME' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_AETIME'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_AETIME' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_AETIMESTAMP.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_AETIMESTAMP' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_AETIMESTAMP'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_AETIMESTAMP' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_ERDAT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERDAT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_ERDAT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_ERDAT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_ERLANGU.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERLANGU' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_ERLANGU'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_ERLANGU' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_ERNAM.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERNAM' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_ERNAM'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_ERNAM' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_ERTIME.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERTIME' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_ERTIME'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_ERTIME' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZ_ERTIMESTAMP.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZ_ERTIMESTAMP' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_ERTIMESTAMP'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_ERTIMESTAMP' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_D0_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to ZL_WRELOCSD_ELOCSD_NEW_D0_CN00 read-only .
protected section.

  methods CREATE_ELOCSD_NEW .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_D0_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).


  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ELOCSD_NEW_D0_CN00'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_D0_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ELOCSD_NEW_D0
  create public .

public section.

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
protected section.

  methods EH_ONZZRESETGPSPOSITION
    importing
      !HTMLB_EVENT type ref to CL_HTMLB_EVENT optional
      !HTMLB_EVENT_EX type ref to IF_HTMLB_DATA optional .

  methods DO_HANDLE_EVENT
    redefinition .
  methods GET_OPTIONS
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_D0_IMPL IMPLEMENTATION.


method DO_HANDLE_EVENT.
    DATA:
        LV_TEXT type string,
        LV_ZZTIDNR_REPLACE Type  ZWR_DCMGM_TIDNR_REPLACE,
        LV_ZZDATE_REPLACE	Type ZWR_DCMGM_DATE_REPLACE.

  data(LR_ELOCSD_NEW) = cast CL_CRM_BOL_ENTITY( TYPED_CONTEXT->ELOCSD_NEW->GET_COLLECTION_WRAPPER( )->GET_CURRENT( ) ).

  if HTMLB_EVENT_EX is bound
    and TO_UPPER( HTMLB_EVENT_EX->EVENT_NAME ) = TO_UPPER( `button` )
    and TO_UPPER( HTMLB_EVENT_EX->EVENT_TYPE ) = TO_UPPER( `click` ) and
    HTMLB_EVENT_EX->EVENT_SERVER_NAME = `ZZRESETGPSPOSITION`. ##NO_TEXT.
      EH_ONZZRESETGPSPOSITION(
        HTMLB_EVENT    = HTMLB_EVENT
        HTMLB_EVENT_EX = HTMLB_EVENT_EX
      ).
  else.
    if HTMLB_EVENT_EX->EVENT_SERVER_NAME = 'EDIT'.
      LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZTIDNR_REPLACE` IV_VALUE = LV_ZZTIDNR_REPLACE ).
      LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZDATE_REPLACE` IV_VALUE = LV_ZZDATE_REPLACE ).
    endif.
    GLOBAL_EVENT = SUPER->DO_HANDLE_EVENT( EVENT           = EVENT
                                           HTMLB_EVENT     = HTMLB_EVENT
                                           HTMLB_EVENT_EX  = HTMLB_EVENT_EX
                                           GLOBAL_MESSAGES = GLOBAL_MESSAGES ).
  endif.
endmethod.


method EH_ONZZRESETGPSPOSITION.
  data(LR_ELOCSD_NEW) = cast CL_CRM_BOL_ENTITY( TYPED_CONTEXT->ELOCSD_NEW->GET_COLLECTION_WRAPPER( )->GET_CURRENT( ) ).
  if LR_ELOCSD_NEW is bound.
    if LR_ELOCSD_NEW->IS_CHANGE_ALLOWED( ).
      if not LR_ELOCSD_NEW->IS_LOCKED( ).
        LR_ELOCSD_NEW->LOCK( IV_REREAD = ABAP_TRUE ).
      endif.
    endif.
    if LR_ELOCSD_NEW->IS_LOCKED( ) and LR_ELOCSD_NEW->IS_CHANGEABLE( ).
      data(LV_ZZLONGITUDE) = value EWAELOCSD_NEW-ZZLONGITUDE( ).
      data(LV_ZZLATITUDE)	= value EWAELOCSD_NEW-ZZLATITUDE( ).
      LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZLONGITUDE` IV_VALUE = LV_ZZLONGITUDE ).
      LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZLATITUDE` IV_VALUE = LV_ZZLATITUDE ).
      /WATP/CL_BOL_MISC=>SAVE_ENTITY(
        PAR_ENTITY = LR_ELOCSD_NEW
        PAR_COMMIT = ABAP_TRUE
      ).

*      data(LR_TX) = LR_ELOCSD_NEW->GET_TRANSACTION( ).
*      LR_TX->SAVE( ).
*      LR_TX->COMMIT( ).
      LR_ELOCSD_NEW->REREAD( ).
    endif.
  endif.
endmethod.


  method GET_OPTIONS.
    ev_editable = ABAP_TRUE.
  endmethod.


method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.
  RT_RESULT = SUPER->IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS( ).
endmethod.


method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.
  DATA:
        LV_TEXT type string,
        LV_ZZTIDNR_REPLACE Type  ZWR_DCMGM_TIDNR_REPLACE,
        LV_ZZDATE_REPLACE	Type ZWR_DCMGM_DATE_REPLACE.

  RT_RESULT = SUPER->IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS( ).

  data(LR_ELOCSD_NEW) = TYPED_CONTEXT->ELOCSD_NEW->GET_COLLECTION_WRAPPER( )->GET_CURRENT( ).
  data(LV_ZZLONGITUDE) = value EWAELOCSD_NEW-ZZLONGITUDE( ).
  data(LV_ZZLATITUDE)	= value EWAELOCSD_NEW-ZZLATITUDE( ).
  data(LV_EQUNR)  = value EWAELOCSD_NEW-EQUNR( ).

  if LR_ELOCSD_NEW is bound.
    LR_ELOCSD_NEW->GET_PROPERTY_AS_VALUE( exporting IV_ATTR_NAME = `ZZLONGITUDE` importing EV_RESULT = LV_ZZLONGITUDE ).
    LR_ELOCSD_NEW->GET_PROPERTY_AS_VALUE( exporting IV_ATTR_NAME = `ZZLATITUDE` importing EV_RESULT = LV_ZZLATITUDE ).
    LR_ELOCSD_NEW->GET_PROPERTY_AS_VALUE( exporting IV_ATTR_NAME = `EQUNR` importing EV_RESULT = LV_EQUNR ).
  endif.
  data(LV_ENABLED) = XSDBOOL( LV_ZZLONGITUDE is not initial or LV_ZZLATITUDE is not initial ).
  message I001(ZWR_NCCMG) into LV_TEXT.
  insert value #( ID       = `ZZRESETGPSPOSITION`
                  TEXT     = LV_TEXT
                  TOOLTIP  = LV_TEXT
                  ON_CLICK = `ZZRESETGPSPOSITION`
                  TYPE     = CL_THTMLB_UTIL=>GC_BUTTON_ICON
                  ENABLED  = LV_ENABLED ) into table RT_RESULT.

  message I002(ZWR_NCCMG) into LV_TEXT.
  LV_ENABLED = XSDBOOL( LV_EQUNR is not initial ).
  if LV_ENABLED = ABAP_TRUE.
    LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZTIDNR_REPLACE` IV_VALUE = LV_ZZTIDNR_REPLACE ).
    LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZDATE_REPLACE` IV_VALUE = LV_ZZDATE_REPLACE ).
    read table RT_RESULT with key ID = 'FORM_EDIT' reference into DATA(LD_EDIT_BUTTON).
    if sy-subrc is initial.
      LD_EDIT_BUTTON->TEXT = LV_TEXT.
      LD_EDIT_BUTTON->TOOLTIP  = LV_TEXT.
    else.
      delete RT_RESULT where ID = 'FORM_EDIT'. ##NO_TEXT
    endif.
  else.
    delete RT_RESULT where ID = 'FORM_EDIT'. ##NO_TEXT
  endif.
      "No Edit allowed:
*  delete RT_RESULT where ID = 'FORM_EDIT'. ##NO_TEXT

*  insert value #( ID       = `ZZREPLACECONTAINER`
*                  TEXT     = LV_TEXT
*                  TOOLTIP  = LV_TEXT
*                  ON_CLICK = `ZZREPLACECONTAINER`
*                  TYPE     = CL_THTMLB_UTIL=>GC_BUTTON_ICON
*                  ENABLED  = LV_ENABLED ) into table RT_RESULT.
endmethod.
ENDCLASS.
class ZL_WRELOCSD_ELOCSD_NEW_DE definition
  public
  inheriting from CL_WCF_BSP_BASE_FORM_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_ELOCSD_NEW_DE_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'ELOCSD_NEW_DETAILS.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_DE IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = ''.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'ELOCSD_NEW'       "#EC NOTEXT
           value  = typed_context->ELOCSD_NEW ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ELOCSD_NEW_DE_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.


class ZL_WRELOCSD_ELOCSD_NEW_DE_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_DE_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.


class ZL_WRELOCSD_ELOCSD_NEW_DE_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to ZL_WRELOCSD_ELOCSD_NEW_DE_CN00 read-only .
protected section.

  methods CREATE_ELOCSD_NEW .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_DE_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).


  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ELOCSD_NEW_DE_CN00'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_DE_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ELOCSD_NEW_DE
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_DE_IMPL IMPLEMENTATION.
ENDCLASS.


class ZL_WRELOCSD_ELOCSD_NEW_O0 definition
  public
  inheriting from CL_WCF_BSP_BASE_OVP
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_ELOCSD_NEW_O0_CTXT read-only .

  methods IP_FR_DIFFCULT
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods DO_DETERMINE_CONFIG_KEYS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_O0 IMPLEMENTATION.


  method DO_DETERMINE_CONFIG_KEYS.

  ev_object_type = 'ZWRELOCSD_DF'.

  endmethod.


  method DO_HANDLE_EVENT.
    DATA:
        LV_ZZTIDNR_REPLACE Type  ZWR_DCMGM_TIDNR_REPLACE,
        LV_ZZDATE_REPLACE	Type ZWR_DCMGM_DATE_REPLACE.

    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'SAVE'.
      if ZCL_WR_CCMG_MISC=>ZCL_HAS_ERRORS( IO_GLOBAL_MESSAGES = global_messages ) = ABAP_FALSE.
        data(LR_ELOCSD_NEW) = cast CL_CRM_BOL_ENTITY( TYPED_CONTEXT->ELOCSD_NEW->GET_COLLECTION_WRAPPER( )->GET_CURRENT( ) ).
        if LR_ELOCSD_NEW is bound.
          LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZTIDNR_REPLACE` IV_VALUE = LV_ZZTIDNR_REPLACE ).
          LR_ELOCSD_NEW->SET_PROPERTY( IV_ATTR_NAME = `ZZDATE_REPLACE` IV_VALUE = LV_ZZDATE_REPLACE ).
          LR_ELOCSD_NEW->REREAD( ).
        endif.
      endif.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = 'X'.
    ev_deleteable = ''.
    ev_main       = 'X'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

    " remove the new button...
    delete rt_result where ID cp '*NEW*'.
  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method IP_FR_DIFFCULT.



  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ELOCSD_NEW_O0_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_O0_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_O0_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_O0_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to ZL_WRELOCSD_ELOCSD_NEW_O0_CN00 read-only .

  methods IF_BSP_WD_HISTORY_STATE~STATE_STORE
    redefinition .
  methods IF_BSP_WD_HISTORY_STATE~STATE_RESTORE
    redefinition .
protected section.

  methods CREATE_ELOCSD_NEW .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_O0_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).


  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ELOCSD_NEW_O0_CN00'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_RESTORE.

    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    state_container->get_item( EXPORTING im_name  = 'ELOCSD_NEW'
                               IMPORTING ex_value = lo_ent
                               EXCEPTIONS OTHERS = 0 ).
    CHECK lo_ent IS BOUND.
    me->elocsd_new->collection_wrapper->clear( ).
    me->elocsd_new->collection_wrapper->add( iv_entity = lo_ent
                                                            iv_set_focus = abap_true ).


  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_STORE.


    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    lo_ent ?= me->elocsd_new->collection_wrapper->get_current( ).
    IF lo_ent IS BOUND.
      state_container->add_item( im_name  = 'ELOCSD_NEW'
                                 im_value =  lo_ent ).
    ENDIF.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_O0_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ELOCSD_NEW_O0
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_O0_IMPL IMPLEMENTATION.
ENDCLASS.
class ZL_WRELOCSD_ELOCSD_NEW_OV definition
  public
  inheriting from CL_WCF_BSP_BASE_OVP
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_ELOCSD_NEW_OV_CTXT read-only .

  methods IP_FR_DIFFCULTS
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods GET_OPTIONS
    redefinition .
  methods DO_DETERMINE_CONFIG_KEYS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_OV IMPLEMENTATION.


  method DO_DETERMINE_CONFIG_KEYS.

  ev_object_type = 'ZWRELOCSD_NEW'.

  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable   = ''.
    ev_deleteable = 'X'.
    ev_main       = 'X'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->ELOCSD_NEW.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method IP_FR_DIFFCULTS.



  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ELOCSD_NEW_OV_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_OV_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEW' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_OV_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_OV_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSD_NEW type ref to ZL_WRELOCSD_ELOCSD_NEW_OV_CN00 read-only .

  methods IF_BSP_WD_HISTORY_STATE~STATE_STORE
    redefinition .
  methods IF_BSP_WD_HISTORY_STATE~STATE_RESTORE
    redefinition .
protected section.

  methods CREATE_ELOCSD_NEW .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_OV_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_ELOCSD_NEW( ).


  endmethod.


  method CREATE_ELOCSD_NEW.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ELOCSD_NEW_OV_CN00'
        model_id       = 'ELOCSD_NEW' ).          "#EC NOTEXT
    ELOCSD_NEW ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_COMPONENT
           iv_name            = ''
           iv_target_node_name = 'ELOCSD_NEW'
           iv_node_2_bind = ELOCSD_NEW ).



  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_RESTORE.

    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    state_container->get_item( EXPORTING im_name  = 'ELOCSD_NEW'
                               IMPORTING ex_value = lo_ent
                               EXCEPTIONS OTHERS = 0 ).
    CHECK lo_ent IS BOUND.
    me->elocsd_new->collection_wrapper->clear( ).
    me->elocsd_new->collection_wrapper->add( iv_entity = lo_ent
                                                            iv_set_focus = abap_true ).


  endmethod.


  method IF_BSP_WD_HISTORY_STATE~STATE_STORE.


    DATA: lo_ent TYPE REF TO if_bol_bo_property_access.

    lo_ent ?= me->elocsd_new->collection_wrapper->get_current( ).
    IF lo_ent IS BOUND.
      state_container->add_item( im_name  = 'ELOCSD_NEW'
                                 im_value =  lo_ent ).
    ENDIF.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ELOCSD_NEW_OV_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ELOCSD_NEW_OV
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ELOCSD_NEW_OV_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO0 definition
  public
  inheriting from CL_AXT_BASE_EMBEDDABLE_VIEW
  create public .

public section.

  interfaces IF_BSP_WD_DYN_BTN_CONTROL .

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_EMBED_ZWRELO0_CTXT read-only .

  methods CONSTRUCTOR .
protected section.

  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'Embed_ZWRELOCSD_NEWQUERY_VS.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO0 IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.

  mv_type = 'DQUERY'.
  mv_anchor_object = 'ZWRELOCSD_NEWQuery$Adv'.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.


  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = 3.

  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'RESULT'       "#EC NOTEXT
           value  = typed_context->RESULT ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_EMBED_ZWRELO0_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO0_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data RESULT type ref to CL_BSP_WD_CONTEXT_NODE read-only .
protected section.

  methods CREATE_RESULT .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO0_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_RESULT( ).


  endmethod.


  method CREATE_RESULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'RESULT' ).          "#EC NOTEXT
    RESULT ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_WINDOW
           iv_name            = 'Embed_ZWRELOCSD_NEWQUERY'
           iv_target_node_name = 'RESULT'
           iv_node_2_bind = RESULT ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO0_IMPL definition
  public
  inheriting from ZL_WRELOCSD_EMBED_ZWRELO0
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO0_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO1 definition
  public
  inheriting from CL_WCF_BSP_BASE_TABLE_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_EMBED_ZWRELO1_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods EH_ONTOAPP
    importing
      !HTMLB_EVENT type ref to CL_HTMLB_EVENT optional
      !HTMLB_EVENT_EX type ref to IF_HTMLB_DATA optional .

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'Embed_ZWRELOCSD_NEWQUERY_Table.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO1 IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!
    WHEN 'TOAPP'.                "#EC NOTEXT
      EH_ONTOAPP( htmlb_event    = htmlb_event
                             htmlb_event_ex = htmlb_event_ex ).


  ENDCASE.
  endmethod.


  method EH_ONTOAPP.


  DATA: lv_index  TYPE i,
        lo_entity           TYPE REF TO CL_CRM_BOL_ENTITY,
        lo_descr            type ref to IF_BOL_BO_PROPERTY_ACCESS,
        lo_navigation       type ref to	if_crm_ui_navigation_service,
        lo_col              TYPE REF TO if_bol_bo_col.

  cl_thtmlb_util=>get_event_info( EXPORTING iv_event = htmlb_event_ex
                                  IMPORTING ev_index = lv_index ).

  IF lv_index is NOT INITIAL.
    lo_entity ?= if_wcf_bsp_view_context~get_main_context_node( )->collection_wrapper->find( iv_index = lv_index ).
  ELSE.
    lo_entity ?= if_wcf_bsp_view_context~get_main_context_node( )->collection_wrapper->get_current( ).
  ENDIF.
  CHECK lo_entity IS BOUND.

  TRY.
      lo_entity = lo_entity->get_related_entity( 'ZWRELOCSD_NEWResult_ZWRELOCSD_NEW' ).
      CHECK lo_entity IS BOUND.
    CATCH cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

  lo_entity = lo_entity->get_root( ).
  CHECK lo_entity IS BOUND.

  CREATE OBJECT lo_col
    TYPE cl_crm_bol_bo_col.

  lo_col->add( lo_entity ).

  lo_descr ?= cl_crm_ui_descriptor_obj_srv=>create_entity_based(
      ir_entity           = lo_entity
      iv_ui_object_action = 'B' "Display
  ).

  CREATE OBJECT lo_col
    TYPE cl_crm_bol_bo_col.

  lo_col->add( lo_descr ).

  lo_navigation = cl_crm_ui_navigation_service=>get_instance( me ).

  lo_navigation->navigate_dynamically( lo_col ).



  endmethod.


  method GET_OPTIONS.

    ev_editable   = ''.
    ev_deleteable = ''.
    ev_min_empty_lines = 0000.
    ev_selection_mode = 'SINGLESELECT'.
    ev_frontend_selection = 'X'.
    ev_frontend_row_creation = abap_true.
    ev_has_detail_page = ''.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->RESULT.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'RESULT'       "#EC NOTEXT
           value  = typed_context->RESULT ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_EMBED_ZWRELO1_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO1_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_TV_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEWResult' ##NO_TEXT.

  methods CONSTRUCTOR .
  methods GET_P_MANDT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZELOCSD_GUID
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
protected section.
private section.

  data MV_X_NAVLINK type ABAP_BOOL value ABAP_TRUE ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO1_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).
    IF cl_axt_ui_services=>is_dynamic_nav_supported(
        iv_ui_object = 'ZWRELOCSD_S'
        iv_action    = 'B'
       ) = abap_false.

      mv_x_navlink = abap_false.
    ENDIF.

  endmethod.


  method GET_P_MANDT.


    CHECK mv_x_navlink = abap_true.
    CASE iv_property.
      WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
        rv_value = if_bsp_dlc_view_descriptor=>field_type_event_link.
        RETURN.
      WHEN if_bsp_wd_model_setter_getter=>fp_onclick.
        rv_value = 'TOAPP'.
        RETURN.
    ENDCASE.


  endmethod.


  method GET_P_ZZELOCSD_GUID.


    CHECK mv_x_navlink = abap_true.
    CASE iv_property.
      WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
        rv_value = if_bsp_dlc_view_descriptor=>field_type_event_link.
        RETURN.
      WHEN if_bsp_wd_model_setter_getter=>fp_onclick.
        rv_value = 'TOAPP'.
        RETURN.
    ENDCASE.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO1_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data RESULT type ref to ZL_WRELOCSD_EMBED_ZWRELO1_CN00 read-only .
protected section.

  methods CREATE_RESULT .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO1_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_RESULT( ).


  endmethod.


  method CREATE_RESULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_EMBED_ZWRELO1_CN00'
        model_id       = 'RESULT' ).          "#EC NOTEXT
    RESULT ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_WINDOW
           iv_name            = 'Embed_ZWRELOCSD_NEWQUERY'
           iv_target_node_name = 'RESULT'
           iv_node_2_bind = RESULT ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO1_IMPL definition
  public
  inheriting from ZL_WRELOCSD_EMBED_ZWRELO1
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO1_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO2 definition
  public
  inheriting from CL_WCF_BSP_BASE_FORM_VIEW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_EMBED_ZWRELO2_CTXT read-only .

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  methods EH_ONTOAPP
    importing
      !HTMLB_EVENT type ref to CL_HTMLB_EVENT optional
      !HTMLB_EVENT_EX type ref to IF_HTMLB_DATA optional .

  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'Embed_ZWRELOCSD_NEWQUERY_Form.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO2 IMPLEMENTATION.


  method CONSTRUCTOR.

  super->constructor( ).
  view_name = view.
  endmethod.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!
    WHEN 'TOAPP'.                "#EC NOTEXT
      EH_ONTOAPP( htmlb_event    = htmlb_event
                             htmlb_event_ex = htmlb_event_ex ).


  ENDCASE.
  endmethod.


  method EH_ONTOAPP.


  DATA: lv_index  TYPE i,
        lo_entity           TYPE REF TO CL_CRM_BOL_ENTITY,
        lo_descr            type ref to IF_BOL_BO_PROPERTY_ACCESS,
        lo_navigation       type ref to	if_crm_ui_navigation_service,
        lo_col              TYPE REF TO if_bol_bo_col.

  cl_thtmlb_util=>get_event_info( EXPORTING iv_event = htmlb_event_ex
                                  IMPORTING ev_index = lv_index ).

  IF lv_index is NOT INITIAL.
    lo_entity ?= if_wcf_bsp_view_context~get_main_context_node( )->collection_wrapper->find( iv_index = lv_index ).
  ELSE.
    lo_entity ?= if_wcf_bsp_view_context~get_main_context_node( )->collection_wrapper->get_current( ).
  ENDIF.
  CHECK lo_entity IS BOUND.

  TRY.
      lo_entity = lo_entity->get_related_entity( 'ZWRELOCSD_NEWResult_ZWRELOCSD_NEW' ).
      CHECK lo_entity IS BOUND.
    CATCH cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

  lo_entity = lo_entity->get_root( ).
  CHECK lo_entity IS BOUND.

  CREATE OBJECT lo_col
    TYPE cl_crm_bol_bo_col.

  lo_col->add( lo_entity ).

  lo_descr ?= cl_crm_ui_descriptor_obj_srv=>create_entity_based(
      ir_entity           = lo_entity
      iv_ui_object_action = 'B' "Display
  ).

  CREATE OBJECT lo_col
    TYPE cl_crm_bol_bo_col.

  lo_col->add( lo_descr ).

  lo_navigation = cl_crm_ui_navigation_service=>get_instance( me ).

  lo_navigation->navigate_dynamically( lo_col ).



  endmethod.


  method GET_OPTIONS.

    ev_editable   = ''.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.

    ro_context_node = typed_context->RESULT.

  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.


  endmethod.


  method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.


  endmethod.


  method SET_MODELS.

*   Set the models for output
  view->set_attribute(
           name   = 'RESULT'       "#EC NOTEXT
           value  = typed_context->RESULT ).
  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_EMBED_ZWRELO2_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO2_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_DEF_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEWResult' ##NO_TEXT.

  methods CONSTRUCTOR .
  methods GET_P_MANDT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZELOCSD_GUID
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
protected section.
private section.

  data MV_X_NAVLINK type ABAP_BOOL value ABAP_TRUE ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO2_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).
    IF cl_axt_ui_services=>is_dynamic_nav_supported(
        iv_ui_object = 'ZWRELOCSD_S'
        iv_action    = 'B'
       ) = abap_false.

      mv_x_navlink = abap_false.
    ENDIF.

  endmethod.


  method GET_P_MANDT.


    CHECK mv_x_navlink = abap_true.
    CASE iv_property.
      WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
        rv_value = if_bsp_dlc_view_descriptor=>field_type_event_link.
        RETURN.
      WHEN if_bsp_wd_model_setter_getter=>fp_onclick.
        rv_value = 'TOAPP'.
        RETURN.
    ENDCASE.


  endmethod.


  method GET_P_ZZELOCSD_GUID.


    CHECK mv_x_navlink = abap_true.
    CASE iv_property.
      WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
        rv_value = if_bsp_dlc_view_descriptor=>field_type_event_link.
        RETURN.
      WHEN if_bsp_wd_model_setter_getter=>fp_onclick.
        rv_value = 'TOAPP'.
        RETURN.
    ENDCASE.


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO2_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data RESULT type ref to ZL_WRELOCSD_EMBED_ZWRELO2_CN00 read-only .
protected section.

  methods CREATE_RESULT .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO2_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_RESULT( ).


  endmethod.


  method CREATE_RESULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_EMBED_ZWRELO2_CN00'
        model_id       = 'RESULT' ).          "#EC NOTEXT
    RESULT ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_WINDOW
           iv_name            = 'Embed_ZWRELOCSD_NEWQUERY'
           iv_target_node_name = 'RESULT'
           iv_node_2_bind = RESULT ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELO2_IMPL definition
  public
  inheriting from ZL_WRELOCSD_EMBED_ZWRELO2
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELO2_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELOC definition
  public
  inheriting from CL_WCF_BSP_BASE_WINDOW
  create public .

public section.

  interfaces IF_WCF_MU .

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_EMBED_ZWRELOC_CTXT read-only .

  methods CALL_OUTBOUND_PLUG
    redefinition .
protected section.

  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELOC IMPLEMENTATION.


  method CALL_OUTBOUND_PLUG.


    DATA: lv_plug_method TYPE seocmpname.

    CONCATENATE 'OP_' iv_outbound_plug INTO lv_plug_method.
    TRANSLATE lv_plug_method TO UPPER CASE.               "#EC SYNTCHAR

    TRY.
        IF iv_data_collection IS BOUND.
          CALL METHOD (lv_plug_method)
            EXPORTING
              iv_data_collection = iv_data_collection.
        ELSE.
          CALL METHOD (lv_plug_method).
        ENDIF.
      CATCH cx_sy_dyn_call_error.
        RAISE EXCEPTION TYPE cx_bsp_wd_incorrect_implement
          EXPORTING
            textid     =
              cx_bsp_wd_incorrect_implement=>window_plug_undefined
            controller = me->view_id
            plug       = iv_outbound_plug.
    ENDTRY.


  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_EMBED_ZWRELOC_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELOC_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data RESULT type ref to CL_BSP_WD_CONTEXT_NODE read-only .
protected section.

  methods CREATE_RESULT .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELOC_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_RESULT( ).


  endmethod.


  method CREATE_RESULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'RESULT' ).          "#EC NOTEXT
    RESULT ?= model.
    CLEAR model.




  endmethod.
ENDCLASS.

class ZL_WRELOCSD_EMBED_ZWRELOC_IMPL definition
  public
  inheriting from ZL_WRELOCSD_EMBED_ZWRELOC
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_EMBED_ZWRELOC_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW definition
  public
  inheriting from CL_WCF_BSP_BASE_WINDOW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_MAINWINDOW_CTXT read-only .

  methods IP_TO_OVERVIEW
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods IP_TO_CREATE
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods CALL_OUTBOUND_PLUG
    redefinition .
protected section.

  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW IMPLEMENTATION.


  method CALL_OUTBOUND_PLUG.


    DATA: lv_plug_method TYPE seocmpname.

    CONCATENATE 'OP_' iv_outbound_plug INTO lv_plug_method.
    TRANSLATE lv_plug_method TO UPPER CASE.               "#EC SYNTCHAR

    TRY.
        IF iv_data_collection IS BOUND.
          CALL METHOD (lv_plug_method)
            EXPORTING
              iv_data_collection = iv_data_collection.
        ELSE.
          CALL METHOD (lv_plug_method).
        ENDIF.
      CATCH cx_sy_dyn_call_error.
        RAISE EXCEPTION TYPE cx_bsp_wd_incorrect_implement
          EXPORTING
            textid     =
              cx_bsp_wd_incorrect_implement=>window_plug_undefined
            controller = me->view_id
            plug       = iv_outbound_plug.
    ENDTRY.


  endmethod.


  method IP_TO_CREATE.



  endmethod.


  method IP_TO_OVERVIEW.



  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_MAINWINDOW_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW0 definition
  public
  inheriting from CL_WCF_BSP_BASE_WINDOW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_MAINWINDOW0_CTXT read-only .

  methods IP_TO_ZWRELOCSD_NEWQUERY_SRC
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods IP_TO_ZWRELOCSD_NEWQUERY_GO
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods IP_F4HELP
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods OP_WCFSELECT
    importing
      !IV_DATA_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods OP_WCFLAUNCH
    importing
      !IV_DATA_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods CALL_OUTBOUND_PLUG
    redefinition .
protected section.

  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW0 IMPLEMENTATION.


  method CALL_OUTBOUND_PLUG.


    DATA: lv_plug_method TYPE seocmpname.

    CONCATENATE 'OP_' iv_outbound_plug INTO lv_plug_method.
    TRANSLATE lv_plug_method TO UPPER CASE.               "#EC SYNTCHAR

    TRY.
        IF iv_data_collection IS BOUND.
          CALL METHOD (lv_plug_method)
            EXPORTING
              iv_data_collection = iv_data_collection.
        ELSE.
          CALL METHOD (lv_plug_method).
        ENDIF.
      CATCH cx_sy_dyn_call_error.
        RAISE EXCEPTION TYPE cx_bsp_wd_incorrect_implement
          EXPORTING
            textid     =
              cx_bsp_wd_incorrect_implement=>window_plug_undefined
            controller = me->view_id
            plug       = iv_outbound_plug.
    ENDTRY.


  endmethod.


  method IP_F4HELP.



  endmethod.


  method IP_TO_ZWRELOCSD_NEWQUERY_GO.



  endmethod.


  method IP_TO_ZWRELOCSD_NEWQUERY_SRC.



  endmethod.


  method OP_WCFLAUNCH.


  DATA:
      lo_bol_desc_entity TYPE REF TO if_bol_bo_property_access,
      lo_desc_entity     TYPE REF TO cl_crm_ui_descriptor_object,
      lo_query_entity    TYPE REF TO if_bol_bo_property_access,
      lo_window          TYPE REF TO cl_bsp_wd_window,
      lo_data_collection TYPE REF TO cl_crm_bol_bo_col,
      lv_follow_up_nav   TYPE string.


*   get first entity from collection
    lo_bol_desc_entity = iv_data_collection->get_first( ).
    CHECK lo_bol_desc_entity IS BOUND.

*   extract query entity from descriptor object
    lo_desc_entity ?= lo_bol_desc_entity.
    lo_query_entity = lo_desc_entity->if_crm_ui_descriptor_object~get_entity( ).

    CASE lo_desc_entity->if_crm_ui_descriptor_object~get_ui_object_action( ).
      WHEN cl_axt_constants=>gc_ui_action_search.
        lv_follow_up_nav = 'TO_ZWRELOCSD_NEWQUERY_SEARCH'.
      WHEN cl_axt_constants=>gc_ui_action_execute.
        lv_follow_up_nav = 'TO_ZWRELOCSD_NEWQUERY_SEARCH_GO'.
    ENDCASE.

    CHECK lv_follow_up_nav is NOT INITIAL.

*   add query entity to data collection for subsequent navigation
    CREATE OBJECT lo_data_collection.
    lo_data_collection->if_bol_bo_col~add( iv_entity = lo_query_entity ).

    lo_window = me->view_manager->get_window_controller( ).
    navigate( source_rep_view = lo_window->get_repository_view( )
              outbound_plug   = lv_follow_up_nav
              data_collection = lo_data_collection ).
  endmethod.


  method OP_WCFSELECT.


    fire_outbound_plug( iv_outbound_plug   = 'WCFSELECT'
                        iv_data_collection = iv_data_collection ).

  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_MAINWINDOW0_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW0_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.
protected section.

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW0_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW0_IMPL definition
  public
  inheriting from ZL_WRELOCSD_MAINWINDOW0
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW0_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW1 definition
  public
  inheriting from CL_WCF_BSP_BASE_WINDOW
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_MAINWINDOW1_CTXT read-only .

  methods IP_TO_EDIT_OVERVIEW
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods IP_TO_OVERVIEW
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .
  methods IP_TO_CREATE
    importing
      !IV_COLLECTION type ref to IF_BOL_BO_COL optional .

  methods CALL_OUTBOUND_PLUG
    redefinition .
protected section.

  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW1 IMPLEMENTATION.


  method CALL_OUTBOUND_PLUG.


    DATA: lv_plug_method TYPE seocmpname.

    CONCATENATE 'OP_' iv_outbound_plug INTO lv_plug_method.
    TRANSLATE lv_plug_method TO UPPER CASE.               "#EC SYNTCHAR

    TRY.
        IF iv_data_collection IS BOUND.
          CALL METHOD (lv_plug_method)
            EXPORTING
              iv_data_collection = iv_data_collection.
        ELSE.
          CALL METHOD (lv_plug_method).
        ENDIF.
      CATCH cx_sy_dyn_call_error.
        RAISE EXCEPTION TYPE cx_bsp_wd_incorrect_implement
          EXPORTING
            textid     =
              cx_bsp_wd_incorrect_implement=>window_plug_undefined
            controller = me->view_id
            plug       = iv_outbound_plug.
    ENDTRY.


  endmethod.


  method IP_TO_CREATE.



  endmethod.


  method IP_TO_EDIT_OVERVIEW.
"#EC NEEDED
* Added by wizard: Inbound plug 'TO_EDIT_OVERVIEW'

  endmethod.


  method IP_TO_OVERVIEW.



  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_MAINWINDOW1_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW1_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.
protected section.

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW1_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW1_IMPL definition
  public
  inheriting from ZL_WRELOCSD_MAINWINDOW1
  create public .

public section.

  methods IP_TO_EDIT_OVERVIEW
    redefinition .
  methods IF_BSP_WD_HISTORY_STATE_DESCR~GET_STATE_DESCRIPTION
    redefinition .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW1_IMPL IMPLEMENTATION.


  method IF_BSP_WD_HISTORY_STATE_DESCR~GET_STATE_DESCRIPTION.
    description = TEXT-001.
  endmethod.


  method IP_TO_EDIT_OVERVIEW.
  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.
protected section.

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_MAINWINDOW_IMPL definition
  public
  inheriting from ZL_WRELOCSD_MAINWINDOW
  create public .

public section.

  methods IF_BSP_WD_HISTORY_STATE_DESCR~GET_STATE_DESCRIPTION
    redefinition .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_MAINWINDOW_IMPL IMPLEMENTATION.


  method IF_BSP_WD_HISTORY_STATE_DESCR~GET_STATE_DESCRIPTION.
    description = text-001.
  endmethod.
ENDCLASS.


class ZL_WRELOCSD_ZWRELOCSD_NE0 definition
  public
  inheriting from CL_WCF_BSP_BASE_SEARCH_QUERY
  create public .

public section.

  interfaces IF_BSP_WD_DYN_BTN_CONTROL .
protected section.

  methods SET_OPTIONS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE0 IMPLEMENTATION.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.


  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = 3.

  endmethod.


  method SET_OPTIONS.

    set_option( iv_option = 'OPTION_SAVED_SEARCHES' iv_value = 'X' ).
    set_option( iv_option = 'OPTION_UI_OBJECT_TYPE' iv_value = 'ZWRELOCSD_S' ).

  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ZWRELOCSD_NE0_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE0_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_SQ_CN00_2
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEWQuery$Adv' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE0_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE0_CN01 definition
  public
  inheriting from CL_WCF_BSP_BASE_SQ_CN01_2
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value '' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE0_CN01 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.


class ZL_WRELOCSD_ZWRELOCSD_NE0_CTXT definition
  public
  inheriting from CL_WCF_BSP_BASE_SQ_CTXT_2
  create public .

public section.
protected section.

  methods CONNECT_NODES
    redefinition .
  methods CREATE_SEARCHQUERYNODE
    redefinition .
  methods CREATE_SEARCHRESULTNODE
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE0_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_SEARCHQUERYNODE.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ZWRELOCSD_NE0_CN00'
        model_id       = 'SEARCHQUERYNODE' ).          "#EC NOTEXT
    SEARCHQUERYNODE ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_CUSTOM
           iv_name            = 'ZWRELOCSD_NEWQUERY_CuCo'
           iv_target_node_name = 'QUERY'
           iv_node_2_bind = SEARCHQUERYNODE ).



  endmethod.


  method CREATE_SEARCHRESULTNODE.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ZWRELOCSD_NE0_CN01'
        model_id       = 'SEARCHRESULTNODE' ).          "#EC NOTEXT
    SEARCHRESULTNODE ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_CUSTOM
           iv_name            = 'ZWRELOCSD_NEWQUERY_CuCo'
           iv_target_node_name = 'RESULT'
           iv_node_2_bind = SEARCHRESULTNODE ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE0_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ZWRELOCSD_NE0
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE0_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE1 definition
  public
  inheriting from CL_WCF_BSP_BASE_SEARCH_RESULT
  create public .

public section.

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
protected section.

  methods OP_SELECT
    redefinition .
  methods GET_OPTIONS
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE1 IMPLEMENTATION.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method GET_OPTIONS.

    ev_editable = ''.
    ev_result_relation = 'ZWRELOCSD_NEWResult_ZWRELOCSD_NEW'.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


  method OP_SELECT.


    DATA: lo_window TYPE REF TO cl_bsp_wd_window.

    lo_window = me->view_manager->get_window_controller( ).
    lo_window->call_outbound_plug( iv_outbound_plug   = 'WCFSELECT'
                                   iv_data_collection = iv_data_collection ).

  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ZWRELOCSD_NE1_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE1_CN00 definition
  public
  inheriting from CL_WCF_BSP_BASE_SRC_RES_CN
  create public .

public section.

  constants BASE_ENTITY_NAME type CRMT_EXT_OBJ_NAME value 'ZWRELOCSD_NEWResult' ##NO_TEXT.

  methods CONSTRUCTOR .
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE1_CN00 IMPLEMENTATION.


  method CONSTRUCTOR.

    super->constructor( ).

  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE1_CTXT definition
  public
  inheriting from CL_WCF_BSP_BASE_SR_CTXT_2
  create public .

public section.

  methods CREATE_SEARCHRESULTNODE
    redefinition .
protected section.

  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE1_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_SEARCHRESULTNODE.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'ZL_WRELOCSD_ZWRELOCSD_NE1_CN00'
        model_id       = 'SEARCHRESULTNODE' ).          "#EC NOTEXT
    SEARCHRESULTNODE ?= model.
    CLEAR model.

    "Binding
    owner->do_context_node_binding(
           iv_controller_type = CL_BSP_WD_CONTROLLER=>CO_TYPE_CUSTOM
           iv_name            = 'ZWRELOCSD_NEWQUERY_CuCo'
           iv_target_node_name = 'RESULT'
           iv_node_2_bind = SEARCHRESULTNODE ).



  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE1_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ZWRELOCSD_NE1
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE1_IMPL IMPLEMENTATION.
ENDCLASS.



class ZL_WRELOCSD_ZWRELOCSD_NE2 definition
  public
  inheriting from CL_BSP_WD_CUSTOM_CONTROLLER
  create public .

public section.

  data TYPED_CONTEXT type ref to ZL_WRELOCSD_ZWRELOCSD_NE2_CTXT read-only .

  methods CONSTRUCTOR .
protected section.

  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants CONTROLLER type STRING value 'ZWRELOCSD_NEWQUERY_CuCo' ##NO_TEXT.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE2 IMPLEMENTATION.


  method CONSTRUCTOR.


    super->constructor( ).
    controller_id = controller.


  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ZWRELOCSD_NE2_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.


class ZL_WRELOCSD_ZWRELOCSD_NE2_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data RESULT type ref to CL_BSP_WD_CONTEXT_NODE read-only .
  data QUERY type ref to CL_BSP_WD_CONTEXT_NODE read-only .
protected section.

  methods CREATE_RESULT .
  methods CREATE_QUERY .

  methods CREATE_CONTEXT_NODES
    redefinition .
  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE2_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.


  method CREATE_CONTEXT_NODES.
 "#EC NEEDED

* create context nodes

    create_RESULT( ).
    create_QUERY( ).


  endmethod.


  method CREATE_QUERY.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'QUERY' ).          "#EC NOTEXT
    QUERY ?= model.
    CLEAR model.




  endmethod.


  method CREATE_RESULT.

    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,"#EC NEEDED
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC NEEDED
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC NEEDED

    model = owner->create_model(
        class_name     = 'CL_BSP_WD_CONTEXT_NODE'
        model_id       = 'RESULT' ).          "#EC NOTEXT
    RESULT ?= model.
    CLEAR model.




  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NE2_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ZWRELOCSD_NE2
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NE2_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NEW definition
  public
  inheriting from CL_WCF_BSP_BASE_SEARCH_VS
  create public .

public section.

  interfaces IF_BSP_WD_DYN_BTN_CONTROL .
protected section.

  methods DO_HANDLE_EVENT
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NEW IMPLEMENTATION.


  method DO_HANDLE_EVENT.


    super->do_handle_event(
      EXPORTING
        event           = event
        htmlb_event     = htmlb_event
        htmlb_event_ex  = htmlb_event_ex
        global_messages = global_messages
      RECEIVING
        global_event    = global_event
      ).

"Eventhandler dispatching
"Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  CASE htmlb_event_ex->event_server_name.
    WHEN 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  ENDCASE.
  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.


  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = 3.

  endmethod.


  method WD_CREATE_CONTEXT.


*   create the context
    context = cl_bsp_wd_context=>get_instance(
          iv_controller = me
          iv_type = 'ZL_WRELOCSD_ZWRELOCSD_NEW_CTXT' ).

    typed_context ?= context.


  endmethod.


  method WD_DESTROY_CONTEXT.


    CLEAR me->typed_context.
    super->wd_destroy_context( ).


  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NEW_CTXT definition
  public
  inheriting from CL_WCF_BSP_BASE_SP_CTXT_2
  create public .

public section.
protected section.

  methods CONNECT_NODES
    redefinition .
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NEW_CTXT IMPLEMENTATION.


  method CONNECT_NODES.
 "#EC NEEDED
    DATA: coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper. "#EC NEEDED




  endmethod.
ENDCLASS.

class ZL_WRELOCSD_ZWRELOCSD_NEW_IMPL definition
  public
  inheriting from ZL_WRELOCSD_ZWRELOCSD_NEW
  create public .

public section.
protected section.
private section.
ENDCLASS.



CLASS ZL_WRELOCSD_ZWRELOCSD_NEW_IMPL IMPLEMENTATION.
ENDCLASS.

class ZL_ZWRELOCS_VSTANDARDCONT definition
  public
  inheriting from CL_WCF_BSP_BASE_FORM_VIEW
  create public .

public section.

  methods CONSTRUCTOR .

  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS
    redefinition .
  methods IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE
    redefinition .
  methods IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE
    redefinition .
protected section.

  data TYPED_CONTEXT type ref to ZL_ZWRELOCS_VSTANDARDCONT_CTXT .

  methods DO_HANDLE_DATA
    redefinition .
  methods DO_HANDLE_EVENT
    redefinition .
  methods GET_OPTIONS
    redefinition .
  methods SET_MODELS
    redefinition .
  methods WD_CREATE_CONTEXT
    redefinition .
  methods WD_DESTROY_CONTEXT
    redefinition .
private section.

  constants VIEW type STRING value 'VStandardContainerInfo.htm' ##NO_TEXT.
ENDCLASS.



CLASS ZL_ZWRELOCS_VSTANDARDCONT IMPLEMENTATION.


method CONSTRUCTOR.
  SUPER->CONSTRUCTOR( ).
  VIEW_NAME = VIEW. "decomment to activate locale view"
endmethod.


method DO_HANDLE_DATA.
  SUPER->DO_HANDLE_DATA( FORM_FIELDS     = FORM_FIELDS
                         GLOBAL_MESSAGES = GLOBAL_MESSAGES ).
endmethod.


method DO_HANDLE_EVENT.
  SUPER->DO_HANDLE_EVENT(
    exporting
      EVENT           = EVENT
      HTMLB_EVENT     = HTMLB_EVENT
      HTMLB_EVENT_EX  = HTMLB_EVENT_EX
      GLOBAL_MESSAGES = GLOBAL_MESSAGES
    receiving
      GLOBAL_EVENT    = GLOBAL_EVENT
    ).

  "Eventhandler dispatching
  "Code slot required to detect events created with workbench
*@SLOT EVENT_DISPATCHER "<-----Do not touch this comment!!!!
  case HTMLB_EVENT_EX->EVENT_SERVER_NAME.
    when 'dummy'.

*@END EVENT_DISPATCHER "<-----Do not touch this comment!!!!


  endcase.
endmethod.


method GET_OPTIONS.
  EV_EDITABLE = ''.
endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.

    rt_result = super->if_bsp_wd_dyn_btn_control~get_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_LOCAL_TOOLBAR_BUTTONS.

    DATA ls_button TYPE crmt_thtmlb_button.
    rt_result = super->if_bsp_wd_dyn_btn_control~get_local_toolbar_buttons( ).

  endmethod.


  method IF_BSP_WD_DYN_BTN_CONTROL~GET_NO_OF_VISIBLE_TOOLBAR_BTNS.

    rv_result = super->if_bsp_wd_dyn_btn_control~get_no_of_visible_toolbar_btns( ).

  endmethod.


method IF_WCF_BSP_VIEW_CONTEXT~GET_MAIN_CONTEXT_NODE.
  RO_CONTEXT_NODE = TYPED_CONTEXT->ELOCSDNEW.
endmethod.


method IF_WCF_BSP_VIEW_CONTEXT~GET_PARENT_CONTEXT_NODE.
endmethod.


method IF_WCF_BSP_VIEW_CONTEXT~GET_ROOT_CONTEXT_NODE.
endmethod.


  method SET_MODELS.
* Set the models for output

* Added by wizard
VIEW->SET_ATTRIBUTE(
      NAME = 'ELOCSDNEW'                                    "#EC NOTEXT
      VALUE  = TYPED_CONTEXT->ELOCSDNEW ).
  endmethod.


  method WD_CREATE_CONTEXT.

*   create the context
    CONTEXT = CL_BSP_WD_CONTEXT=>GET_INSTANCE(
            IV_CONTROLLER = ME
            IV_TYPE = 'ZL_ZWRELOCS_VSTANDARDCONT_CTXT' ).

    TYPED_CONTEXT ?= CONTEXT.


  endmethod.


  method WD_DESTROY_CONTEXT.

    clear ME->TYPED_CONTEXT.
    SUPER->WD_DESTROY_CONTEXT( ).

  endmethod.
ENDCLASS.

class ZL_ZWRELOCS_VSTANDARDCONT_CN00 definition
  public
  inheriting from ZL_WRELOCSD_ELOCSD_NEW_D0_CN00
  create public .

public section.

  methods GET_ZZCONTAINER_TEXT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZLOOKUP
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZSALES_PERSON_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZSERVTYPE_DEF
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_FAHRER_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_GEFABF_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods GET_ZZ_INTERNE_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(VALUE) type STRING .
  methods SET_ZZCONTAINER_TEXT
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZLOOKUP
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZSALES_PERSON_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_FAHRER_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_GEFABF_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods SET_ZZ_INTERNE_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
      !VALUE type STRING .
  methods GET_M_ZZ_INTERNE_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_GEFABF_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZ_FAHRER_NOTIZ
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZSALES_PERSON_DESCR
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZLOOKUP
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_M_ZZCONTAINER_TEXT
    importing
      !ATTRIBUTE_PATH type STRING
    returning
      value(METADATA) type ref to IF_BSP_METADATA_SIMPLE .
  methods GET_I_ZZ_INTERNE_NOTIZ
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_GEFABF_NOTIZ
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZ_FAHRER_NOTIZ
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZSALES_PERSON_DESCR
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZLOOKUP
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_I_ZZCONTAINER_TEXT
    importing
      !ITERATOR type ref to IF_BOL_BO_COL_ITERATOR optional
    returning
      value(RV_DISABLED) type STRING .
  methods GET_V_ZZOWNERSHIP_CONTAINER
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_V_ZZSALES_PERSON
    importing
      !IV_MODE type CHAR1 default IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE
      !IV_INDEX type I optional
    returning
      value(RV_VALUEHELP_DESCRIPTOR) type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
  methods GET_P_ZZ_INTERNE_NOTIZ
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_GEFABF_NOTIZ
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_FAHRER_NOTIZ
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_ERTIMESTAMP
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_ERTIME
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_ERNAM
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_ERLANGU
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_ERDAT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_AETIMESTAMP
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_AETIME
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_AENAM
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZ_AEDAT
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .
  methods GET_P_ZZOWNERSHIP_CONTAINER
    importing
      !IV_PROPERTY type STRING
      !IV_INDEX type I optional
      !IV_DISPLAY_MODE type ABAP_BOOL default ABAP_FALSE
    returning
      value(RV_VALUE) type STRING .

  methods GET_I_ZZSALES_PERSON
    redefinition .
protected section.

  data FVH_OWNERSHIP type ref to IF_BSP_WD_VALUEHELP_DESCRIPTOR .
private section.
ENDCLASS.



CLASS ZL_ZWRELOCS_VSTANDARDCONT_CN00 IMPLEMENTATION.


  method GET_I_ZZCONTAINER_TEXT.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZCONTAINER_TEXT' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  method GET_I_ZZLOOKUP.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

  TRY.

        IF current->is_property_readonly(
                      'ZZLOOKUP' ) = abap_false. "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.



  endmethod.


  METHOD GET_I_ZZSALES_PERSON.

    AUTHORITY-CHECK OBJECT 'ZWR_CRM_CM'
      ID 'ACTVT' FIELD '02'.

    IF SY-SUBRC <> 0.
      RV_DISABLED = 'TRUE'.
    ENDIF.

  ENDMETHOD.


  METHOD get_i_zzsales_person_descr.
    DATA: current TYPE REF TO if_bol_bo_property_access.

    rv_disabled = 'TRUE'.
*    if iterator is bound.
*      current = iterator->get_current( ).
*    else.
*      current = collection_wrapper->get_current( ).
*    endif.
*
*  TRY.
*
*        IF current->is_property_readonly(
*                      'ZZCONTAINER_TEXT' ) = abap_false. "#EC NOTEXT
*          rv_disabled = 'FALSE'.
*        ENDIF.
*
*    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
*          cx_crm_genil_model_error.
*      RETURN.
*  ENDTRY.

  ENDMETHOD.


  METHOD get_i_zz_fahrer_notiz.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: coll   TYPE REF TO if_bol_entity_col.
    DATA: entity TYPE REF TO cl_crm_bol_entity.

    rv_disabled = 'TRUE'.
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
          iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ) ##NO_TEXT.
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        IF current->is_property_readonly(
                      'ZZ_FAHRER_NOTIZ' ) = abap_false.     "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

      CATCH cx_sy_ref_is_initial
            cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.



  ENDMETHOD.


  METHOD get_i_zz_gefabf_notiz.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: coll   TYPE REF TO if_bol_entity_col.
    DATA: entity TYPE REF TO cl_crm_bol_entity.

    rv_disabled = 'TRUE'.
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
          iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ) ##NO_TEXT.
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        IF current->is_property_readonly(
                      'ZZ_GEFABF_NOTIZ' ) = abap_false.     "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

      CATCH cx_sy_ref_is_initial
            cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.



  ENDMETHOD.


  METHOD get_i_zz_interne_notiz.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: coll   TYPE REF TO if_bol_entity_col.
    DATA: entity TYPE REF TO cl_crm_bol_entity.

    rv_disabled = 'TRUE'.
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
          iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ) ##NO_TEXT.
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        IF current->is_property_readonly(
                      'ZZ_INTERNE_NOTIZ' ) = abap_false.    "#EC NOTEXT
          rv_disabled = 'FALSE'.
        ENDIF.

      CATCH cx_sy_ref_is_initial
            cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.



  ENDMETHOD.


  method GET_M_ZZCONTAINER_TEXT.

  DATA: attr    TYPE ZWR_DCMGM_CUST_CONT_TXT.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZCONTAINER_TEXT'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  method GET_M_ZZLOOKUP.

  DATA: attr    TYPE TEXT1000.

  DATA: dref    TYPE REF TO data.

  GET REFERENCE OF attr INTO dref.

  metadata ?= if_bsp_model_binding~get_attribute_metadata(
       attribute_ref  = dref
       attribute_path = attribute_path
       name           = 'ZZLOOKUP'  "#EC NOTEXT
*      COMPONENT      =
       no_getter      = 1 ).


  endmethod.


  METHOD get_m_zzsales_person_descr.

    DATA: attr    TYPE text100.

    DATA: dref    TYPE REF TO data.

    GET REFERENCE OF attr INTO dref.

    metadata ?= if_bsp_model_binding~get_attribute_metadata(
         attribute_ref  = dref
         attribute_path = attribute_path
         name           = 'ZZSALES_PERSON_DESCR'            "#EC NOTEXT
*      COMPONENT      =
         no_getter      = 1 ).


  ENDMETHOD.


  METHOD get_m_zz_fahrer_notiz.

    DATA: attr    TYPE zwr_dcmgm_fahrer_notiz.

    DATA: dref    TYPE REF TO data.

    GET REFERENCE OF attr INTO dref.

    metadata ?= if_bsp_model_binding~get_attribute_metadata(
         attribute_ref  = dref
         attribute_path = attribute_path
         name           = 'ZZ_FAHRER_NOTIZ'                 "#EC NOTEXT
*      COMPONENT      =
         no_getter      = 1 ).


  ENDMETHOD.


  METHOD get_m_zz_gefabf_notiz.

    DATA: attr    TYPE zwr_dcmgm_gefabf_notiz.

    DATA: dref    TYPE REF TO data.

    GET REFERENCE OF attr INTO dref.

    metadata ?= if_bsp_model_binding~get_attribute_metadata(
         attribute_ref  = dref
         attribute_path = attribute_path
         name           = 'ZZ_GEFABF_NOTIZ'                 "#EC NOTEXT
*      COMPONENT      =
         no_getter      = 1 ).


  ENDMETHOD.


  METHOD get_m_zz_interne_notiz.

    DATA: attr    TYPE zwr_dcmgm_interne_notiz.

    DATA: dref    TYPE REF TO data.

    GET REFERENCE OF attr INTO dref.

    metadata ?= if_bsp_model_binding~get_attribute_metadata(
         attribute_ref  = dref
         attribute_path = attribute_path
         name           = 'ZZ_INTERNE_NOTIZ'                "#EC NOTEXT
*      COMPONENT      =
         no_getter      = 1 ).


  ENDMETHOD.


METHOD GET_P_ZZOWNERSHIP_CONTAINER.
  VIEW_CONTEXT_PROP_PICKLIST.
ENDMETHOD.


METHOD GET_P_ZZ_AEDAT.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD GET_P_ZZ_AENAM.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD GET_P_ZZ_AETIME.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD GET_P_ZZ_AETIMESTAMP.

 CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.


ENDMETHOD.


METHOD GET_P_ZZ_ERDAT.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD GET_P_ZZ_ERLANGU.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD GET_P_ZZ_ERNAM.

 CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD GET_P_ZZ_ERTIME.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  endcase.

ENDMETHOD.


METHOD GET_P_ZZ_ERTIMESTAMP.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


METHOD get_p_zz_fahrer_notiz.

  CASE iv_property.
*   Set the field type
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_textarea.
  ENDCASE.

ENDMETHOD.


METHOD get_p_zz_gefabf_notiz.

  CASE iv_property.
*   Set the field type
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_textarea.
  ENDCASE.

ENDMETHOD.


METHOD get_p_zz_interne_notiz.

  CASE iv_property.
*   Set the field type
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_textarea.
  ENDCASE.

ENDMETHOD.


method GET_V_ZZOWNERSHIP_CONTAINER.
  if FVH_OWNERSHIP is not bound.
    FVH_OWNERSHIP = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_PLD_DOMAIN(
      PAR_DOMAINNAME = 'ZWR_CMGM_CONTAINER_OWNER'
      PAR_EMPTY_LINE = ABAP_TRUE ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_OWNERSHIP.

endmethod.


METHOD get_v_zzsales_person.

  rv_valuehelp_descriptor = NEW cl_bsp_wd_valuehelp_f4descr(
   iv_help_id        = 'ZWR_VKNK_CONTACT'
   iv_help_id_kind   = if_bsp_wd_valuehelp_f4descr=>help_id_kind_comp
   iv_input_mapping  = VALUE if_bsp_wd_valuehelp_f4descr=>gtype_param_mapping_tab(
                         ( context_attr = 'STRUCT.ZZSALES_PERSON' f4_attr = 'PARNR' ) )
   iv_output_mapping = VALUE if_bsp_wd_valuehelp_f4descr=>gtype_param_mapping_tab(
                         ( context_attr = 'STRUCT.ZZSALES_PERSON' f4_attr = 'PARNR' )
                         ( context_attr = 'STRUCT.ZZSALES_PERSON_DESCR' f4_attr = 'NAME_TEXT' ) )
   iv_object_ref     = me ) ##NO_TEXT.

ENDMETHOD.


  method GET_ZZCONTAINER_TEXT.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'."#EC NOTEXT


    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.


  TRY.

    TRY.
        dref = current->get_property( 'ZZCONTAINER_TEXT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZCONTAINER_TEXT not bound'."#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> type DATA.
        assign dref->* to <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
          value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                  "#EC NOTEXT
    ENDTRY.


  endmethod.


  METHOD GET_ZZLOOKUP.

    DATA: CURRENT TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS.
    DATA: DREF    TYPE REF TO DATA.
    DATA: LR_ADRUNCANR TYPE REF TO DATA.

    VALUE =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF ITERATOR IS BOUND.
      CURRENT = ITERATOR->GET_CURRENT( ).
    ELSE.
      CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
    ENDIF.


    TRY.

        TRY.
            LR_ADRUNCANR = CURRENT->GET_PROPERTY( 'ZZADRUNCANR' ). "#EC NOTEXT
          CATCH CX_CRM_CIC_PARAMETER_ERROR.
        ENDTRY.

        IF LR_ADRUNCANR IS NOT BOUND.
          CLEAR VALUE.
          RETURN.
        ENDIF.

        SELECT SINGLE A~UNIDENT, A~UNNAME, A~ADRCLASSNR, A~ADRVPGRPNR, A~ADRTUCONR,
                               T~PRINTTEXT1, T~PRINTTEXT2, T~PRINTTEXT3
                   FROM /WATP/TADRUNCA AS A
             INNER JOIN /WATP/TADRUNCAT AS T
                     ON A~ADRUNCANR = T~ADRUNCANR
                    AND T~SPRAS = @SY-LANGU
                  WHERE A~ADRUNCANR = @LR_ADRUNCANR->*
                   INTO @DATA(LS_ADRUNCA).

        IF SY-SUBRC <> 0.
          CLEAR VALUE.
          RETURN.
        ENDIF.

        DATA(LV_ID)       = |UN{ LS_ADRUNCA-UNIDENT }{ LS_ADRUNCA-UNNAME }|.
        DATA(LV_PRINTTXT) = |({ LS_ADRUNCA-PRINTTEXT1 }, { LS_ADRUNCA-PRINTTEXT2 }, { LS_ADRUNCA-PRINTTEXT3 })|.
        DATA(LV_END)      = |{ LS_ADRUNCA-ADRCLASSNR }, { LS_ADRUNCA-ADRVPGRPNR }{ LS_ADRUNCA-ADRTUCONR }|.
        DATA(LV_RESULT)   = |{ LV_ID }, { LV_PRINTTXT }, { LV_END }|.


      CATCH CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
            CX_CRM_GENIL_MODEL_ERROR.
        RETURN.
    ENDTRY.


    TRY.
        VALUE = LV_RESULT.
      CATCH CX_BSP_CONV_ILLEGAL_REF.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        VALUE = '-CURR/QUANT REF DATA MISSING-'.
      CATCH CX_ROOT.
        VALUE = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.

  ENDMETHOD.


  METHOD get_zzsales_person_descr.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.


    value =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.


    TRY.

        TRY.
            dref = current->get_property( 'ZZSALES_PERSON_DESCR' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZZSALES_PERSON_DESCR  not bound'. "#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> TYPE data.
        ASSIGN dref->* TO <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.


  ENDMETHOD.


  METHOD GET_ZZSERVTYPE_DEF.

    DATA: CURRENT TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS.
    DATA: DREF    TYPE REF TO DATA.


    VALUE =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF ITERATOR IS BOUND.
      CURRENT = ITERATOR->GET_CURRENT( ).
    ELSE.
      CURRENT = COLLECTION_WRAPPER->GET_CURRENT( ).
    ENDIF.


    TRY.

        TRY.
            DREF = CURRENT->GET_PROPERTY( 'ZZSERVTYPE_DEF' ). "#EC NOTEXT
          CATCH CX_CRM_CIC_PARAMETER_ERROR.
        ENDTRY.

      CATCH CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
            CX_CRM_GENIL_MODEL_ERROR.
        RETURN.
    ENDTRY.

    IF DREF IS NOT BOUND.

      VALUE = 'ZWRELOCSD_NEW/ZZSERVTYPE_DEF not bound'.     "#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        VALUE = IF_BSP_MODEL_UTIL~CONVERT_TO_STRING( DATA_REF = DREF
                                                     ATTRIBUTE_PATH = ATTRIBUTE_PATH
                                                    ).

        IF VALUE IS INITIAL.
          VALUE = 'Z2'.
        ENDIF.
      CATCH CX_BSP_CONV_ILLEGAL_REF.
        FIELD-SYMBOLS: <L_DATA> TYPE DATA.
        ASSIGN DREF->* TO <L_DATA>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        VALUE = '-CURR/QUANT REF DATA MISSING-'.
      CATCH CX_ROOT.
        VALUE = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.


  ENDMETHOD.


  METHOD GET_ZZ_FAHRER_NOTIZ.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.
    DATA: coll   TYPE REF TO if_bol_entity_col.
    DATA: entity TYPE REF TO cl_crm_bol_entity.

    value =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.


    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
                 iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ). "#EC NOTEXT
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        TRY.
            dref = current->get_property( 'ZZ_FAHRER_NOTIZ' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZWRELOCSD_LTXT_NOTE/Z... not bound'.   "#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> TYPE data.
        ASSIGN dref->* TO <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.


  ENDMETHOD.


  METHOD GET_ZZ_GEFABF_NOTIZ.

    DATA: CURRENT_PARENT TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS.
    DATA: CURRENT_CHILD  TYPE REF TO IF_BOL_BO_PROPERTY_ACCESS.

    DATA: LR_NOTIZ     TYPE REF TO DATA.
    DATA: LR_ADRUNCANR TYPE REF TO DATA.

    DATA: COLL   TYPE REF TO IF_BOL_ENTITY_COL.
    DATA: ENTITY TYPE REF TO CL_CRM_BOL_ENTITY.

    VALUE =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF ITERATOR IS BOUND.
      CURRENT_PARENT = ITERATOR->GET_CURRENT( ).
    ELSE.
      CURRENT_PARENT = COLLECTION_WRAPPER->GET_CURRENT( ).
    ENDIF.


    TRY.

        ENTITY ?= CURRENT_PARENT.
        COLL = ENTITY->GET_RELATED_ENTITIES( IV_RELATION_NAME = 'ZWRELOCSD_LTXT_NOTE' ). "#EC NOTEXT
        CURRENT_CHILD = COLL->GET_CURRENT( ).
        IF CURRENT_CHILD IS NOT BOUND.
          RETURN.
        ENDIF.

        TRY.
            LR_NOTIZ = CURRENT_CHILD->GET_PROPERTY( 'ZZ_GEFABF_NOTIZ' ). "#EC NOTEXT
          CATCH CX_CRM_CIC_PARAMETER_ERROR.
        ENDTRY.

      CATCH CX_SY_REF_IS_INITIAL CX_SY_MOVE_CAST_ERROR
            CX_CRM_GENIL_MODEL_ERROR.
        RETURN.
    ENDTRY.

    IF LR_NOTIZ IS NOT BOUND.

      VALUE = 'ZWRELOCSD_NEW/ZWRELOCSD_LTXT_NOTE/Z... not bound'. "#EC NOTEXT

      RETURN.
    ENDIF.

    TRY.

        IF LR_NOTIZ->* IS NOT INITIAL.
          VALUE = IF_BSP_MODEL_UTIL~CONVERT_TO_STRING( DATA_REF = LR_NOTIZ
                                                       ATTRIBUTE_PATH = ATTRIBUTE_PATH ).
        ELSE.

          TRY.
              LR_ADRUNCANR = CURRENT_PARENT->GET_PROPERTY( 'ZZADRUNCANR' ). "#EC NOTEXT
            CATCH CX_CRM_CIC_PARAMETER_ERROR.
          ENDTRY.

          IF LR_ADRUNCANR IS NOT BOUND.
            CLEAR VALUE.
            RETURN.
          ENDIF.

          SELECT SINGLE A~UNIDENT, A~UNNAME, A~ADRCLASSNR, A~ADRVPGRPNR, A~ADRTUCONR,
                        T~PRINTTEXT1, T~PRINTTEXT2, T~PRINTTEXT3
            FROM /WATP/TADRUNCA AS A
      INNER JOIN /WATP/TADRUNCAT AS T
              ON A~ADRUNCANR = T~ADRUNCANR
             AND T~SPRAS = @SY-LANGU
           WHERE A~ADRUNCANR = @LR_ADRUNCANR->*
            INTO @DATA(LS_ADRUNCA).

          IF SY-SUBRC <> 0.
            CLEAR VALUE.
            RETURN.
          ENDIF.

          DATA(LV_ID)       = |UN{ LS_ADRUNCA-UNIDENT }{ LS_ADRUNCA-UNNAME }|.
          DATA(LV_PRINTTXT) = |({ LS_ADRUNCA-PRINTTEXT1 }, { LS_ADRUNCA-PRINTTEXT2 }, { LS_ADRUNCA-PRINTTEXT3 })|.
          DATA(LV_END)      = |{ LS_ADRUNCA-ADRCLASSNR }, { LS_ADRUNCA-ADRVPGRPNR }{ LS_ADRUNCA-ADRTUCONR }|.
          DATA(LV_RESULT)   = |{ LV_ID }, { LV_PRINTTXT }, { LV_END }|.
          VALUE = LV_RESULT.

        ENDIF.
      CATCH CX_BSP_CONV_ILLEGAL_REF.
        FIELD-SYMBOLS: <L_DATA> TYPE DATA.
        ASSIGN LR_NOTIZ->* TO <L_DATA>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        VALUE = '-CURR/QUANT REF DATA MISSING-'.
      CATCH CX_ROOT.
        VALUE = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.


  ENDMETHOD.


  METHOD GET_ZZ_INTERNE_NOTIZ.

    DATA: current TYPE REF TO if_bol_bo_property_access.
    DATA: dref    TYPE REF TO data.
    DATA: coll   TYPE REF TO if_bol_entity_col.
    DATA: entity TYPE REF TO cl_crm_bol_entity.

    value =
'ZWRELOCSD_NEW not bound'.                                  "#EC NOTEXT


    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.


    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
                 iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ). "#EC NOTEXT
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        TRY.
            dref = current->get_property( 'ZZ_INTERNE_NOTIZ' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.

    IF dref IS NOT BOUND.

      value = 'ZWRELOCSD_NEW/ZWRELOCSD_LTXT_NOTE/Z... not bound'.   "#EC NOTEXT

      RETURN.
    ENDIF.
    TRY.
        value = if_bsp_model_util~convert_to_string( data_ref = dref
                                    attribute_path = attribute_path ).
      CATCH cx_bsp_conv_illegal_ref.
        FIELD-SYMBOLS: <l_data> TYPE data.
        ASSIGN dref->* TO <l_data>.
*       please implement here some BO specific handler coding
*       conversion of currency/quantity field failed caused by missing
*       unit relation
*       Coding sample:
*       provide currency, decimals, and reference type
*       value = cl_bsp_utility=>make_string(
*                          value = <l_data>
*                          reference_value = c_currency
*                          num_decimals = decimals
*                          reference_type = reference_type
*                          ).
        value = '-CURR/QUANT REF DATA MISSING-'.
      CATCH cx_root.
        value = '-CONVERSION FAILED-'.                      "#EC NOTEXT
    ENDTRY.


  ENDMETHOD.


  method SET_ZZCONTAINER_TEXT.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZCONTAINER_TEXT' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZCONTAINER_TEXT'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZCONTAINER_TEXT' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  method SET_ZZLOOKUP.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE ANY,
      <oval> TYPE ANY.

*   get current entity
    if iterator is bound.
      current = iterator->get_current( ).
    else.
      current = collection_wrapper->get_current( ).
    endif.

*   get old value and dataref to appropriate type

  TRY.

    TRY.
        dref = current->get_property( 'ZZLOOKUP' ). "#EC NOTEXT
      CATCH cx_crm_cic_parameter_error.
    ENDTRY.

    CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
          cx_crm_genil_model_error.
      RETURN.
  ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZLOOKUP'."#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZLOOKUP' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  endmethod.


  METHOD set_zzsales_person_descr.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data.

    FIELD-SYMBOLS:
      <nval> TYPE any,
      <oval> TYPE any.

*   get current entity
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

*   get old value and dataref to appropriate type

    TRY.

        TRY.
            dref = current->get_property( 'ZZSALES_PERSON_DESCR ' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZSALES_PERSON_DESCR '. "#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZSALES_PERSON_DESCR ' "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  ENDMETHOD.


  METHOD set_zz_fahrer_notiz.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data,
      coll    TYPE REF TO if_bol_entity_col,
      entity  TYPE REF TO cl_crm_bol_entity.

    FIELD-SYMBOLS:
      <nval> TYPE any,
      <oval> TYPE any.

*   get current entity
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

*   get old value and dataref to appropriate type

    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
                 iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ). "#EC NOTEXT
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        TRY.
            dref = current->get_property( 'ZZ_FAHRER_NOTIZ' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_FAHRER_NOTIZ'. "#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_FAHRER_NOTIZ'      "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  ENDMETHOD.


  METHOD set_zz_gefabf_notiz.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data,
      coll    TYPE REF TO if_bol_entity_col,
      entity  TYPE REF TO cl_crm_bol_entity.

    FIELD-SYMBOLS:
      <nval> TYPE any,
      <oval> TYPE any.

*   get current entity
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

*   get old value and dataref to appropriate type

    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
                 iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ). "#EC NOTEXT
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        TRY.
            dref = current->get_property( 'ZZ_GEFABF_NOTIZ' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_GEFABF_NOTIZ'. "#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_GEFABF_NOTIZ'      "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  ENDMETHOD.


  METHOD set_zz_interne_notiz.
    DATA:
      current TYPE REF TO if_bol_bo_property_access,
      dref    TYPE REF TO data,
      copy    TYPE REF TO data,
      coll    TYPE REF TO if_bol_entity_col,
      entity  TYPE REF TO cl_crm_bol_entity.

    FIELD-SYMBOLS:
      <nval> TYPE any,
      <oval> TYPE any.

*   get current entity
    IF iterator IS BOUND.
      current = iterator->get_current( ).
    ELSE.
      current = collection_wrapper->get_current( ).
    ENDIF.

*   get old value and dataref to appropriate type

    TRY.

        entity ?= current.
        coll = entity->get_related_entities(
                 iv_relation_name = 'ZWRELOCSD_LTXT_NOTE' ). "#EC NOTEXT
        current = coll->get_current( ).
        IF current IS NOT BOUND.
          RETURN.
        ENDIF.

        TRY.
            dref = current->get_property( 'ZZ_INTERNE_NOTIZ' ). "#EC NOTEXT
          CATCH cx_crm_cic_parameter_error.
        ENDTRY.

      CATCH cx_sy_ref_is_initial cx_sy_move_cast_error
            cx_crm_genil_model_error.
        RETURN.
    ENDTRY.


*   assure that attribue exists
    CHECK dref IS BOUND.

*   set <oval> to old value
    ASSIGN dref->* TO <oval>.
*   create a copy for new value
    CREATE DATA copy LIKE <oval>.
*   set <nval> to new value
    ASSIGN copy->* TO <nval>.

*   fill new value using the right conversion
    TRY.
*        TRY.
        CALL METHOD if_bsp_model_util~convert_from_string
          EXPORTING
            data_ref       = copy
            value          = value
            attribute_path = attribute_path.
*        CATCH cx_bsp_conv_illegal_ref.
*          FIELD-SYMBOLS: <l_data> type DATA.
*          assign copy->* to <l_data>.
*         please implement here some BO specific handler coding
*         conversion of currency/quantity field failed caused by missing
*         unit relation
*         Coding sample:
*         provide currency for currency fields or decimals for quantity (select from T006).
*          cl_bsp_utility=>instantiate_simple_data(
*                             value = value
*                             reference = c_currency
*                             num_decimals = decimals
*                             use_bsp_exceptions = abap_true
*                             data = <l_data> ).
*      ENDTRY.
      CATCH cx_sy_conversion_error.
        RAISE EXCEPTION TYPE cx_bsp_conv_failed
          EXPORTING
            name = 'ZZ_INTERNE_NOTIZ'. "#EC NOTEXT
    ENDTRY.

*   only set new value if value has changed
    IF <nval> <> <oval>.

      current->set_property(
                      iv_attr_name = 'ZZ_INTERNE_NOTIZ'     "#EC NOTEXT
                      iv_value     = <nval> ).

    ENDIF.


  ENDMETHOD.
ENDCLASS.

class ZL_ZWRELOCS_VSTANDARDCONT_CTXT definition
  public
  inheriting from CL_BSP_WD_CONTEXT
  create public .

public section.

  data ELOCSDNEW type ref to ZL_ZWRELOCS_VSTANDARDCONT_CN00 read-only .
protected section.

  methods CREATE_ELOCSDNEW .

  methods CONNECT_NODES
    redefinition .
  methods CREATE_CONTEXT_NODES
    redefinition .
private section.

  data ZELOCSDNEW type ref to ZL_ZWRELOCS_VSTANDARDCONT_CN00 .
ENDCLASS.



CLASS ZL_ZWRELOCS_VSTANDARDCONT_CTXT IMPLEMENTATION.


  method CONNECT_NODES.

    data: COLL_WRAPPER type ref to CL_BSP_WD_COLLECTION_WRAPPER.


  endmethod.


  method CREATE_CONTEXT_NODES.
* create context nodes

* Added by wizard
CREATE_ELOCSDNEW( ).
  endmethod.


  method CREATE_ELOCSDNEW.
    DATA:
      model        TYPE REF TO if_bsp_model,
      coll_wrapper TYPE REF TO cl_bsp_wd_collection_wrapper,
      entity       TYPE REF TO cl_crm_bol_entity,           "#EC *
      entity_col   TYPE REF TO if_bol_entity_col.           "#EC *

    model = owner->create_model(

        class_name     = 'ZL_ZWRELOCS_VSTANDARDCONT_CN00'

        model_id       = 'ELOCSDNEW' ).                     "#EC NOTEXT
    elocsdnew ?= model.
    CLEAR model.

* Added by wizard
* bind Context Node to controller
    owner->do_context_node_binding(
    iv_controller_type = cl_bsp_wd_controller=>co_type_component
    iv_target_node_name = 'ELOCSD_NEW'
    iv_node_2_bind = elocsdnew
    iv_mode = cl_bsp_wd_controller=>co_share_collection ).
  endmethod.
ENDCLASS.

class ZL_ZWRELOCS_VSTANDARDCONT_IMPL definition
  public
  inheriting from ZL_ZWRELOCS_VSTANDARDCONT
  create public .

public section.
protected section.

  methods GET_OPTIONS
    redefinition .
  methods DO_PREPARE_OUTPUT
    redefinition .
private section.
ENDCLASS.



CLASS ZL_ZWRELOCS_VSTANDARDCONT_IMPL IMPLEMENTATION.


  METHOD do_prepare_output.
    super->do_prepare_output( iv_first_time = iv_first_time ).

    DATA(lo_elocsdnew) = me->typed_context->elocsdnew.
    IF lo_elocsdnew IS BOUND.
      DATA(lo_entity) = lo_elocsdnew->collection_wrapper->get_current( ).
      IF lo_entity IS BOUND.
        DATA(lv_sales_person) = lo_entity->get_property_as_string( 'ZZSALES_PERSON' ).
      ENDIF.
    ENDIF.

    IF lv_sales_person IS INITIAL.
      RETURN.
    ENDIF.

    DATA(lv_parnr) = VALUE parnr( ).
    lv_parnr = |{ lv_sales_person ALPHA = IN }|.

    SELECT SINGLE name_text
      FROM zvwr_contact
     WHERE parnr = @lv_parnr
      INTO @DATA(lv_name).

    IF sy-subrc <> 0.
      RETURN.
    ENDIF.

    lo_entity->set_property_as_string( iv_attr_name = 'ZZSALES_PERSON_DESCR'
                                       iv_value     =  |{ lv_name }| ).

  ENDMETHOD.


method GET_OPTIONS.
  EV_EDITABLE = ABAP_TRUE.
endmethod.
ENDCLASS.


