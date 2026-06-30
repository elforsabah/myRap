<img width="1880" height="811" alt="image" src="https://github.com/user-attachments/assets/630af298-150f-49d1-953b-7ffd5831407e" />

<img width="1150" height="592" alt="image" src="https://github.com/user-attachments/assets/bc88f8a9-405d-4aab-8665-7edf3aed9bce" />


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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_CONTSHARE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_CONT_COUNT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_H_ZZDATE_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_HIDDEN                      TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_H_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_HIDDEN                      TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_AB
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_AB.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_BIS
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_BIS.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_CONTSHARE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_CONTSHARE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_CONT_COUNT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_CONT_COUNT.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_EQUNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_EQUNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_IBASE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_IBASE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ILOAN
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_ILOAN.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_KUNNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_KUNNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_LOGIKNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_LOGIKNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_MATNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_MATNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_OBKNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_OBKNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_OBZAE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_OBZAE.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_POSNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_POSNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_SERVFREQNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_SERVFREQNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_SERVLOC
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_SERVLOC.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_TIMFR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_TIMFR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_TIMTO
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_TIMTO.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_VBELN
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_VBELN.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZADRUNCANR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_ZZADRUNCANR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZADRVPGRPNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_ZZADRVPGRPNR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZAVVCODE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZBILL_BLOCK
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZCONTASSIGNMENT_ID
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_ZZCONTASSIGNMENT_ID.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZDATE_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZINVOICE_SPLIT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZLATITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZLONGITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZNO_PORTAL
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZPO_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZREQUESTER_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZSALES_PERSON
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZTIDNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZWASTE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZWASTE_DEF_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_I_ZZWASTE_DEF_DESCR.
  RV_DISABLED = 'TRUE' ##NO_TEXT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_AEDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_AENAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_AETIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_AETIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_ERDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_ERLANGU
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_ERNAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_ERTIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_I_ZZ_ERTIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] RV_DISABLED                    TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_CONTSHARE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_CONT_COUNT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZADRUNCANR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZADRVPGRPNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZAVVCODE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZBILL_BLOCK
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZCONTASSIGNMENT_ID
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZDATE_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZINVOICE_SPLIT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZLATITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZLONGITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZNO_PORTAL
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZPO_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZREQUESTER_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZSALES_PERSON
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZTIDNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZWASTE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZWASTE_DEF_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_AEDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_AENAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_AETIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_AETIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_ERDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_ERLANGU
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_ERNAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_ERTIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_M_ZZ_ERTIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [<-()] METADATA                       TYPE REF TO IF_BSP_METADATA_SIMPLE
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_CONTSHARE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_CONT_COUNT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZAVVCODE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZAVVCODE_DEF.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      "rv_value = cl_bsp_dlc_view_descriptor=>field_type_input.

    "WHEN IF_BSP_WD_MODEL_SETTER_GETTER=>...
      "...
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZBILL_BLOCK
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_P_ZZBILL_BLOCK.
  VIEW_CONTEXT_PROP_PICKLIST.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZINVOICE_SPLIT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_P_ZZINVOICE_SPLIT.
  VIEW_CONTEXT_PROP_PICKLIST.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZNO_PORTAL
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_P_ZZNO_PORTAL.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZPO_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_P_ZZPO_MANDAT.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZREQUESTER_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_P_ZZREQUESTER_MANDAT.
  VIEW_CONTEXT_PROP_CHECKBOX.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZSERVTYPE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZSERVTYPE_DEF.
  VIEW_CONTEXT_PROP_PICKLIST.
ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_P_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_PROPERTY                    TYPE        STRING
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [--->] IV_DISPLAY_MODE                TYPE        ABAP_BOOL (default =ABAP_FALSE)
* | [<-()] RV_VALUE                       TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
METHOD GET_P_ZZTIDNR_REPLACE.

  CASE iv_property.
    WHEN if_bsp_wd_model_setter_getter=>fp_fieldtype.
      "rv_value = cl_bsp_dlc_view_descriptor=>field_type_input.

    "WHEN IF_BSP_WD_MODEL_SETTER_GETTER=>...
      "...
  ENDCASE.

ENDMETHOD.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZAVVCODE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZBILL_BLOCK
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZINVOICE_SPLIT
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
method GET_V_ZZINVOICE_SPLIT.
  if FVH_ZZINVOICE_SPLIT is not bound.
    FVH_ZZINVOICE_SPLIT = CL_CRM_EEWA_BSP_SEARCHHELPER=>CL_CREATE_PLD_DOMAIN(
      PAR_DOMAINNAME = 'ZWR_CMGM_INVOICE_SPLIT'
      PAR_EMPTY_LINE = ABAP_TRUE ) ##NO_TEXT.
  endif.
  RV_VALUEHELP_DESCRIPTOR = FVH_ZZINVOICE_SPLIT.
endmethod.


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZROUTE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZSERVTYPE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZWASTE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_V_ZZWDF_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] IV_MODE                        TYPE        CHAR1 (default =IF_BSP_WD_MODEL_SETTER_GETTER=>RUNTIME_MODE)
* | [--->] IV_INDEX                       TYPE        I(optional)
* | [<-()] RV_VALUEHELP_DESCRIPTOR        TYPE REF TO IF_BSP_WD_VALUEHELP_DESCRIPTOR
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZADRUNCANR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZADRVPGRPNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZAVVCODE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZBILL_BLOCK
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZCONTASSIGNMENT_ID
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZDATE_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZINVOICE_SPLIT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZLATITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZLONGITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZNO_PORTAL
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZPO_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZREQUESTER_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZSALES_PERSON
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZTIDNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZWASTE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZWASTE_DEF_DESCR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_AEDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_AENAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_AETIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_AETIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_ERDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_ERLANGU
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_ERNAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_ERTIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->GET_ZZ_ERTIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [<-()] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_CONTSHARE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_CONT_COUNT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZAVVCODE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZBILL_BLOCK
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZDATE_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZINVOICE_SPLIT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZLATITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZLONGITUDE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZNO_PORTAL
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZPO_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZREQUESTER_MANDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZSALES_PERSON
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZTIDNR
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZTIDNR_REPLACE
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZWASTE_DEF
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_AEDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_AENAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_AETIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_AETIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_ERDAT
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_ERLANGU
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_ERNAM
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_ERTIME
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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


* <SIGNATURE>---------------------------------------------------------------------------------------+
* | Instance Public Method ZL_WRELOCSD_ELOCSD_NEW_D0_CN00->SET_ZZ_ERTIMESTAMP
* +-------------------------------------------------------------------------------------------------+
* | [--->] ATTRIBUTE_PATH                 TYPE        STRING
* | [--->] ITERATOR                       TYPE REF TO IF_BOL_BO_COL_ITERATOR(optional)
* | [--->] VALUE                          TYPE        STRING
* +--------------------------------------------------------------------------------------</SIGNATURE>
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
