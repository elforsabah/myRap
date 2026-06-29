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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZCONTAINER_TEXT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZLOOKUP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZSALES_PERSON
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
  METHOD GET_I_ZZSALES_PERSON.

    AUTHORITY-CHECK OBJECT 'ZWR_CRM_CM'
      ID 'ACTVT' FIELD '02'.

    IF SY-SUBRC <> 0.
      RV_DISABLED = 'TRUE'.
    ENDIF.

  ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZSALES_PERSON_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZ_FAHRER_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZ_GEFABF_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_I_ZZ_INTERNE_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_M_ZZCONTAINER_TEXT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_M_ZZLOOKUP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_M_ZZSALES_PERSON_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_M_ZZ_FAHRER_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_M_ZZ_GEFABF_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_M_ZZ_INTERNE_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZOWNERSHIP_CONTAINER
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZOWNERSHIP_CONTAINER.
  VIEW_CONTEXT_PROP_PICKLIST.
ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_AEDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_AEDAT.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_AENAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_AENAM.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_AETIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_AETIME.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_AETIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_AETIMESTAMP.

 CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.


ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_ERDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_ERDAT.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_ERLANGU
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_ERLANGU.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_ERNAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_ERNAM.

 CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_ERTIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_ERTIME.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  endcase.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_ERTIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZ_ERTIMESTAMP.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_text.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_FAHRER_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD get_p_zz_fahrer_notiz.

  CASE iv_property.
*   Set the field type
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_textarea.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_GEFABF_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD get_p_zz_gefabf_notiz.

  CASE iv_property.
*   Set the field type
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_textarea.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_P_ZZ_INTERNE_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD get_p_zz_interne_notiz.

  CASE iv_property.
*   Set the field type
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      rv_value = cl_bsp_dlc_view_descriptor=>field_type_textarea.
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_V_ZZOWNERSHIP_CONTAINER
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_V_ZZOWNERSHIP_CONTAINER.
  if FVH_OWNERSHIP is not bound.
    FVH_OWNERSHIP = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_PLD_DOMAIN(
      PAR_DOMAINNAME = 'ZWR_CMGM_CONTAINER_OWNER'
      PAR_EMPTY_LINE = ABAP_TRUE ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_OWNERSHIP.

endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_V_ZZSALES_PERSON
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZCONTAINER_TEXT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZLOOKUP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZSALES_PERSON_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZSERVTYPE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZ_FAHRER_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZ_GEFABF_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->GET_ZZ_INTERNE_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->SET_ZZCONTAINER_TEXT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->SET_ZZLOOKUP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->SET_ZZSALES_PERSON_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->SET_ZZ_FAHRER_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->SET_ZZ_GEFABF_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_ZWRELOCS_VSTANDARDCONT_CN00->SET_ZZ_INTERNE_NOTIZ
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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
