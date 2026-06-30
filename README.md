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



<img width="953" height="824" alt="image" src="https://github.com/user-attachments/assets/84839dc1-af37-4dbc-9395-b7476a3315ae" />


function /WATP/CMW_SHLP.
*"----------------------------------------------------------------------
*"*"Lokale Schnittstelle:
*"  TABLES
*"      SHLP_TAB TYPE  SHLP_DESCT
*"      RECORD_TAB STRUCTURE  SEAHLPRES
*"  CHANGING
*"     REFERENCE(SHLP) TYPE  SHLP_DESCR
*"     REFERENCE(CALLCONTROL) LIKE  DDSHF4CTRL STRUCTURE  DDSHF4CTRL
*"----------------------------------------------------------------------

  data:
    LCHAR type CHAR30,
    LTABLE type ref to DATA,
    LREQTYPE type CRM_EWAEL_REQUESTTYPE value 'R',
    LSTRUCTNAME type STRUNAME,
    LFIELDPROP type ref to DDSHFPROP,
    LFLD_TAB type table of DFIES,
    LTEXT type STRING,
    LRESKEY type STRING,
    LSELOPT type ref to DDSHSELOPT,
    LRESOLVER type ref to OBJECT,
    LHELPER type ref to IF_CRM_EEWA_SHLPHELPER,
    LEX type ref to CX_ROOT,
    LPROPERTY   type CRM_EWA_EL_PROPS,
    LPROPERTIES type CRM_ISU_WAEL_PROPS.

  field-symbols:
    <LTABLE> type standard table.

  SEARCHHELP_CALL_BADI.

  try.
      case CALLCONTROL-STEP.
        when 'SELECT'.
* workaround to fix checkbox values
          delete SHLP-SELOPT
            where SIGN = 'I' and OPTION = 'EQ'
              and LOW = '<IMG CLASS=TH-SAPCB-IMG TITLE="" ALT="" SRC="'.
* check for resolver
          read table SHLP-SELOPT reference into LSELOPT
            with key SHLPFIELD = IF_BSP_WD_CUSTOM_F4_CALLBACK=>GC_KEY_REF.
          if SY-SUBRC = 0.
            LRESKEY = LSELOPT->LOW.
            LRESOLVER = CL_BSP_WD_REFERENCE_TOOL=>GET_REFERENCE(
              IV_CONTROLLER_KEY = LRESKEY
            ).
            ASSIGNCAST LHELPER LRESOLVER.
          endif.
          if LHELPER is not initial.
            LHELPER->PREPARE(
              changing
                PAR_SHLP = SHLP
            ).
            LHELPER->GET_PROPERTIES(
              importing
                PAR_PROPERTIES = LPROPERTIES
            ).
          endif.
* Establish result table structure
          loop at SHLP-FIELDPROP reference into LFIELDPROP.
            if LFIELDPROP->FIELDNAME = 'RES_STRUCTNAME'.
              LSTRUCTNAME = LFIELDPROP->DEFAULTVAL.
              if LSTRUCTNAME is initial.
                select single FIELDVALUE
                  into LSTRUCTNAME
                  from TCRM_EWA_EL_SHLP
                  where SHLPNAME = SHLP-SHLPNAME
                    and FIELDNAME = LFIELDPROP->FIELDNAME.
              endif.
              replace all occurrences of '''' in LSTRUCTNAME
                with SPACE.
              if LSTRUCTNAME is initial.
                RAISE_STATIC CX_CRM_EEWA_BASE SHLP_NO_STRUCTNAME.
              endif.
              create data LTABLE type standard table of (LSTRUCTNAME).
              assign LTABLE->* to <LTABLE>.
            elseif LFIELDPROP->FIELDNAME = 'REQ_REQTYPE'.
              LCHAR = LFIELDPROP->DEFAULTVAL.
              replace all occurrences of '''' in LCHAR
                with SPACE.
              LREQTYPE = LCHAR.
            endif.
          endloop.
* Request type switch
          case LREQTYPE.
            when 'L'.
              CL_CRM_EEWA_SHLP=>LOCALSELECT(
                exporting
                  PAR_CALLCONTROL = CALLCONTROL
                  PAR_SHLP        = SHLP
                  PAR_STRUCTNAME  = LSTRUCTNAME
                  PAR_PROPS       = LPROPERTIES
                changing
                  PAR_TABLE       = <LTABLE>
              ).
            when 'R'.
              read table LPROPERTIES with key NAME = 'MAXROWS' transporting no fields.
              if SY-SUBRC is not initial.
                LPROPERTY-NAME = 'MAXROWS'.
                LPROPERTY-VALUE = 100.
                insert LPROPERTY into table LPROPERTIES.
              endif.
              CL_CRM_EEWA_SHLP=>REMOTESELECT(
                exporting
                  PAR_CALLCONTROL = CALLCONTROL
                  PAR_SHLP        = SHLP
                  PAR_STRUCTNAME  = LSTRUCTNAME
                  PAR_PROPS       = LPROPERTIES
                changing
                  PAR_TABLE       = <LTABLE>
              ).
          endcase.
* Prepare table for display
          sort <LTABLE>.
          call function 'DDIF_NAMETAB_GET'
            exporting
              TABNAME   = LSTRUCTNAME
            tables
              DFIES_TAB = LFLD_TAB
            exceptions
              others    = 0.

          perform MAP_ITAB_TO_RECORD_TAB in program SAPLSDSD
            tables
              <LTABLE>
            using
              SHLP-FIELDDESCR
              LFLD_TAB
            changing
              RECORD_TAB[].

          CALLCONTROL-STEP = 'DISP'.
* Don't process STEP DISP additionally in this call
          return.
      endcase.
    catch CX_ROOT into LEX.                              "#EC CATCH_ALL
* Dump because no other exception handling is above the function module which will show the error.
      LTEXT = LEX->GET_TEXT( ).
      message LTEXT type 'X'.
  endtry.
endfunction.
