REPORT rewaorder_download MESSAGE-ID ewa.
************************************************************************
*        REPORT REWAORDER_DOWNLOAD
*----------------------------------------------------------------------*
* - report prepares waste order download
*   ( EWAORDERDOWN )
************************************************************************
INCLUDE: emsg,
         iewmodes,
         iewaranges,
         ieawaste.
*---------------------------------------------------------------------*
*  BADI
*---------------------------------------------------------------------*
CLASS cl_exithandler DEFINITION LOAD.
DATA exit_hz TYPE REF TO if_ex_isu_wa_hazard_waste.

TABLES: ewaorderd.
DATA:   fieldvalue TYPE shvalue_d .
DATA:   return_tab TYPE ddshretval OCCURS 0 WITH HEADER LINE,
        activetab  TYPE fcode.
DATA:   program    TYPE program,
        dynpro     TYPE dynnr,
        exit       TYPE REF TO if_ex_isu_wa_order_select.
DATA:   ok_code   like sy-ucomm.
DATA: t LIKE itcpp-tddest OCCURS 1.


INCLUDE eewa_order_selection_screen.

* selection of period
SELECTION-SCREEN BEGIN OF BLOCK period WITH FRAME TITLE text-s02.
SELECT-OPTIONS: period   FOR ewaorderd-period  MODIF ID per OBLIGATORY.
SELECTION-SCREEN END OF BLOCK period.


* type of output
SELECTION-SCREEN BEGIN OF BLOCK output WITH FRAME TITLE text-s14.
* output all waste orders
*SELECTION-SCREEN BEGIN OF LINE.
PARAMETER       f_all LIKE ewaorderd-f_allorder RADIOBUTTON GROUP radi .
* SELECTION-SCREEN COMMENT 3(29) text-s15.
PARAMETER       forder   LIKE ewaorderd-form_allord .
*SELECTION-SCREEN END OF LINE.
* output waste order cover page
*SELECTION-SCREEN BEGIN OF LINE.
PARAMETERS f_cover LIKE ewaorderd-f_headorder
           RADIOBUTTON GROUP radi.
*SELECTION-SCREEN COMMENT 3(29) text-s16.
PARAMETERS       fcover   LIKE ewaorderd-form_headord.
*SELECTION-SCREEN END OF LINE.
* output waste order positions
*SELECTION-SCREEN BEGIN OF LINE.
PARAMETERS f_orders  LIKE ewaorderd-f_posorder
           RADIOBUTTON GROUP radi.
*SELECTION-SCREEN COMMENT 3(29) text-s17.
PARAMETERS       fpos    LIKE ewaorderd-form_posord.
*SELECTION-SCREEN END OF LINE.
SELECTION-SCREEN END OF BLOCK output.

* selection of print parameters
PARAMETERS: printpar LIKE eprintparams NO-DISPLAY.
PARAMETERS: ehsprpar  LIKE  pri_params NO-DISPLAY.
PARAMETERS: PRDISDOC type ISUWA_T_EPRINTPARAMS NO-DISPLAY.
SELECTION-SCREEN BEGIN OF BLOCK archive WITH FRAME TITLE text-s13.
PARAMETERS: para_set AS CHECKBOX MODIF ID par.
PARAMETERS: para_ehs AS CHECKBOX MODIF ID par.
SELECTION-SCREEN END OF BLOCK archive.

* Pushbutton Jobeinplanung
SELECTION-SCREEN PUSHBUTTON /1(25)  text-s05 USER-COMMAND job.

PARAMETERS:   run_no   LIKE balhdr-extnumber NO-DISPLAY.
PARAMETERS:   notfound LIKE regen-kennzx NO-DISPLAY.
PARAMETERS:   PARA_TAB TYPE FCODE NO-DISPLAY.

*---------------------------------------------------------------------
*$*$ SELECTION-SCREEN OUTPUT
AT SELECTION-SCREEN OUTPUT.
* control field input
  clear ok_code .
  LOOP AT SCREEN.
    IF screen-group1 = 'PAR'.
      screen-output = '1'.
      screen-input  = '0'.
      MODIFY SCREEN.
    ENDIF.

  ENDLOOP.
  IF   f_orders IS INITIAL .
    LOOP AT SCREEN.
      IF screen-group1 = 'POS'.
        screen-output = '1'.
        screen-input  = '0'.
        IF  screen-group3 = 'LOW' OR
            screen-group3 = 'HGH' .
          screen-output = '0'.
        ENDIF.
        MODIFY SCREEN.
      ENDIF.
    ENDLOOP.
  ELSE.
    LOOP AT SCREEN.
      IF screen-group1 = 'POS' AND
         screen-group3 NE 'OPU'.
        screen-output = '1'.
        screen-input  = '1'.
        MODIFY SCREEN.
      ENDIF.
    ENDLOOP.
  ENDIF .

*---------------------------------------------------------------------
*$*$ SELECTION-SCREEN ON VALUE-REQUEST
AT SELECTION-SCREEN ON VALUE-REQUEST FOR orderpos-low.

  DESCRIBE TABLE order.
  IF ( sy-tfill > 1 OR order-high > order-low OR sy-tfill = 0 OR
     order-low = space ) OR f_orders IS INITIAL.
    MESSAGE i628(ewa).
    EXIT.
  ENDIF.

* F4 Hilfe für Auftragsposition.
  CALL FUNCTION 'F4IF_FIELD_VALUE_REQUEST'
    EXPORTING
      tabname           = 'EWA_ORDER_OBJECT'
      fieldname         = 'ORDER_LAUFNR'
      searchhelp        = 'EWAORDERPOS'
      shlpparam         = ' '
      multiple_choice   = 'X'
    TABLES
      return_tab        = return_tab
    EXCEPTIONS
      field_not_found   = 1
      no_help_for_field = 2
      inconsistent_help = 3
      no_values_found   = 4
      OTHERS            = 5.

  CHECK  NOT return_tab[] IS INITIAL.
  REFRESH orderpos.
  orderpos-sign = 'I'.
  orderpos-option = 'EQ'.
  LOOP AT return_tab WHERE fieldname = 'ORDER_LAUFNR'.
    orderpos-low = return_tab-fieldval.
    APPEND orderpos  .
  ENDLOOP.
  READ TABLE orderpos INDEX 1.
*
AT SELECTION-SCREEN ON FCOVER.
  if not FCOVER is initial.
     select single FORMKEY from EFRM into FCOVER
        where FORMKEY = FCOVER  and
              FORMCLASS = 'IS_U_WA_ORDER_HEAD_DOWNLOAD'.
     if sy-subrc ne 0.
      message e388(ez) with FCOVER 'IS_U_WA_ORDER_HEAD_DOWNLOAD'.
      clear FCOVER.
     endif.
  endif.

AT SELECTION-SCREEN ON FORDER.
  if not  FORDER  is initial.
     select single FORMKEY from EFRM into FORDER
        where FORMKEY = FORDER  and
              FORMCLASS = 'IS_U_WA_ORDER_DOWNLOAD'.
     if sy-subrc ne 0.
      message e388(ez) with FORDER 'IS_U_WA_ORDER_DOWNLOAD'.
      clear FORDER.
     endif.
  endif.

AT SELECTION-SCREEN ON FPOS.

  if not  FPOS is initial.
     select single FORMKEY from EFRM into FPOS
        where FORMKEY = FPOS  and
              FORMCLASS = 'IS_U_WA_ORDER_OBJECT_DOWNLOAD'.
     if sy-subrc ne 0.
      message e388(ez) with FPOS 'IS_U_WA_ORDER_OBJECT_DOWNLOAD'.
      clear FPOS.
     endif.
  endif.

AT SELECTION-SCREEN ON RADIOBUTTON GROUP radi.

  IF NOT f_orders IS INITIAL.
    LOOP AT SCREEN.
      IF screen-group1 = 'POS' AND
         screen-group3 NE 'OPU'.
        screen-output = '1'.
        screen-input  = '1'.
        MODIFY SCREEN.
      ENDIF.
    ENDLOOP.
  ELSE.
    REFRESH orderpos .
    LOOP AT SCREEN.
      IF screen-group1 = 'POS'.
        screen-output = '1'.
        screen-input  = '0'.
        IF  screen-group3 = 'LOW' OR
            screen-group3 = 'HGH' .
          screen-output = '0'.
        ENDIF.
        MODIFY SCREEN.
      ENDIF.
    ENDLOOP.
  ENDIF.
*---------------------------------------------------------------------
*$*$ INITIALIZATION
INITIALIZATION.

  tabs1 = text-s19.
  tabs2 = text-s03.
  tabs3 = text-s04.
  tabs4 = text-s20.

  CALL FUNCTION 'RS_SET_SELSCREEN_STATUS'
    EXPORTING
      p_status  = 'SEL'
    TABLES
      p_exclude = t
    EXCEPTIONS
      OTHERS    = 1.
  IF ( sy-subrc = 1 ).
    mac_msg_putx co_msg_programming_error '898' 'E9' 'sy-subrc'
                 'RS_SET_SELSCREEN_STATUS' space space space.
    IF 1 = 2.
*  nur wegen Verwendungsnachweis
      MESSAGE a898(e9) WITH space space space space.
    ENDIF.
  ENDIF.

* setzen des Periodenanfangs auf Tagesdatum
  period-low = sy-datum + 1.
  APPEND period.

  FREE MEMORY ID co_prog_order_downld.

*$*$ AT SELECTION-SCREEN------------------------------------------------
AT SELECTION-SCREEN.

  IF exit_hz IS INITIAL.
    CALL METHOD cl_exithandler=>get_instance
      exporting
        EXIT_NAME = 'ISU_WA_HAZARD_WASTE'
        NULL_INSTANCE_ACCEPTED = 'X'
      CHANGING
        instance = exit_hz.
  ENDIF.

  if sy-batch = ABAP_FALSE.
    PARA_TAB = tabbl-activetab.
  else.
    "wenn PARA_TAB nicht in der Variante
    if PARA_TAB is INITIAL.
      PARA_TAB = tabbl-activetab. "TABS1
    endif.
  endif.

  if SY-DYNNR = '1000'.
    check sy-ucomm ne ok_code .
    ok_code = sy-ucomm .
  CASE sy-ucomm .
    WHEN 'JOB'.
*   Hintergrundjob einplanen
      CALL FUNCTION 'ISU_WAORDER_CREATE_BATCH_JOB'
        EXPORTING
          x_jobname     = 'WASTE_ORDER_PREPARATION'
          x_report_name = 'REWAORDER_DOWNLOAD'.
    WHEN 'PARA'.
      CALL FUNCTION 'ISU_WAORDER_GET_PRINT_PARAM'
        CHANGING
          xy_printpar = printpar
          xy_para_set = para_set.

    WHEN 'PARA_EHS'.

      PERFORM EXIT_ACTION_PINTPARAMS.
*      IF 1 = 2 .
*        PERFORM get_print_param_ehs  .
*      ENDIF.
    WHEN 'ONLI'.
      CLEAR sy-ucomm.
      CALL FUNCTION 'ISU_WAORDER_CHECK_PRINT_PARAM'
        CHANGING
          xy_printpar = printpar
          xy_para_set = para_set.
      IF para_set IS INITIAL.
        EXIT.
      ENDIF.
  ENDCASE.
  endif.

  ACTIVETAB = PARA_TAB.
  INCLUDE eewa_order_build_ranges.

  EXPORT date_low = period-low  TO MEMORY ID 'LOW'.
  EXPORT date_high = period-high  TO MEMORY ID 'HIGH'.

  CALL FUNCTION 'ISU_WAORDER_CHECK_SELSCREEN'
    EXPORTING
      land           = land
      x_wmode        = co_create
      x_active_tab   = activetab
    CHANGING
      xy_selopt      = select_options
    EXCEPTIONS
      not_found      = 1
      internal_error = 2
      not_authorized = 3
      user_canceled  = 4
      not_customized = 5
      foreign_lock   = 6
      invalid_key    = 7
      number_error   = 8
      system_error   = 9
      input_error    = 10
      OTHERS         = 11.
  IF sy-subrc <> 0.
    MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
            WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
  ENDIF.

************************************************************************
*                                                                      *
*                  M A I N     P R O G R A M M                         *
*                                                                      *
************************************************************************
*$*$ START OF SELECTION
START-OF-SELECTION.

  CHECK para_set = 'X'.

  CALL FUNCTION 'ISU_WAORDER_SELSCREEN_EXECUTE'
    EXPORTING
      x_wmode        = co_execute
      x_land         = land
      x_printpar     = printpar
      x_printpar_ehs = ehsprpar
      x_prdisdoc    = prdisdoc
      x_formall      = forder
      x_formitems    = fpos
      x_formcover    = fcover
      x_flag_all     = f_all
      x_flag_items   = f_orders
      x_flag_cover   = f_cover
      x_active_tab   = activetab
      X_EXIT_HZ      = exit_hz
    CHANGING
      xy_selopt      = select_options
    EXCEPTIONS
      not_found      = 1
      internal_error = 2
      not_authorized = 3
      user_canceled  = 4
      not_customized = 5
      foreign_lock   = 6
      invalid_key    = 7
      number_error   = 8
      system_error   = 9
      input_error    = 10
      OTHERS         = 11.
  IF sy-subrc <> 0.
    MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
            WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
  ENDIF.

  INCLUDE aorder_download_get_print_pf01.

  INCLUDE aorder_download_status_0100o01.

  INCLUDE aorder_download_user_commani01.



  *----------------------------------------------------------------------*
***INCLUDE AORDER_DOWNLOAD_GET_PRINT_PF01 .
*----------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*&      Form  get_print_param_ehs
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
*  -->  p1        text
*  <--  p2        text
*----------------------------------------------------------------------*
form get_print_param_ehs .

  DATA: l_valid type c .

  CALL FUNCTION 'GET_PRINT_PARAMETERS'
    EXPORTING
*     ARCHIVE_ID                   = C_CHAR_UNKNOWN
*     ARCHIVE_INFO                 = C_CHAR_UNKNOWN
*     ARCHIVE_MODE                 = C_CHAR_UNKNOWN
*     ARCHIVE_TEXT                 = C_CHAR_UNKNOWN
*     AR_OBJECT                    = C_CHAR_UNKNOWN
*     ARCHIVE_REPORT               = C_CHAR_UNKNOWN
*     AUTHORITY                    = C_CHAR_UNKNOWN
*     COPIES                       = C_NUM3_UNKNOWN
*     COVER_PAGE                   = C_CHAR_UNKNOWN
*     DATA_SET                     = C_CHAR_UNKNOWN
*     DEPARTMENT                   = C_CHAR_UNKNOWN
      DESTINATION                  = EHSPRPAR-PDEST
*     EXPIRATION                   = C_NUM1_UNKNOWN
*     IMMEDIATELY                  = C_CHAR_UNKNOWN
*     IN_ARCHIVE_PARAMETERS        = ' '
*     IN_PARAMETERS                = ' '
*     LAYOUT                       = C_CHAR_UNKNOWN
*     LINE_COUNT                   = C_INT_UNKNOWN
*     LINE_SIZE                    = C_INT_UNKNOWN
*     LIST_NAME                    = C_CHAR_UNKNOWN
*     LIST_TEXT                    = C_CHAR_UNKNOWN
*     MODE                         = ' '
*     NEW_LIST_ID                  = C_CHAR_UNKNOWN
*     NO_DIALOG                    = C_FALSE
*     RECEIVER                     = C_CHAR_UNKNOWN
*     RELEASE                      = C_CHAR_UNKNOWN
*     REPORT                       = C_CHAR_UNKNOWN
*     SAP_COVER_PAGE               = C_CHAR_UNKNOWN
*     HOST_COVER_PAGE              = C_CHAR_UNKNOWN
*     PRIORITY                     = C_NUM1_UNKNOWN
*     SAP_OBJECT                   = C_CHAR_UNKNOWN
*     TYPE                         = C_CHAR_UNKNOWN
*     USER                         = SY-UNAME
    IMPORTING
*     OUT_ARCHIVE_PARAMETERS       =
      OUT_PARAMETERS               = EHSPRPAR "EHS_PRINT_PARAMETERS
      VALID                        = para_ehs
    EXCEPTIONS
     ARCHIVE_INFO_NOT_FOUND       = 1
     INVALID_PRINT_PARAMS         = 2
     INVALID_ARCHIVE_PARAMS       = 3
     OTHERS                       = 4
            .
  IF SY-SUBRC <> 0.
         MESSAGE ID SY-MSGID TYPE SY-MSGTY NUMBER SY-MSGNO
         WITH SY-MSGV1 SY-MSGV2 SY-MSGV3 SY-MSGV4.
  ELSE.
    para_ehs = 'X'.
  ENDIF.

*  perform move_print_ehs using     para_ehs
*                         changing  PRINTPARAMS .


endform.                    " get_print_param_ehs


****************************************************************
*&
*&        FORM EXIT_ACTION_PINTPARAMS.
*&
****************************************************************
FORM EXIT_ACTION_PINTPARAMS.
  DATA: MSGLIST  type EWA_BAPIRETURN1_TAB ,
        MSGLINE  type bapireturn1 .


  IF exit_hz IS INITIAL.
    CALL METHOD cl_exithandler=>get_instance
      EXPORTING
        EXIT_NAME              = 'ISU_WA_HAZARD_WASTE'
        NULL_INSTANCE_ACCEPTED = 'X'
      CHANGING
        instance               = exit_hz.
  ENDIF.

  CALL METHOD EXIT_HZ->ACTION_PINTPARAMS
    EXPORTING
      OBJECT_ID   =  IF_EX_ISU_WA_HAZARD_WASTE=>COBJ_WAORDER
    IMPORTING
      MSGLIST     =  MSGLIST
    CHANGING
      PRINTPARAMS = PRDISDOC
      .

   read table MSGLIST into msgline with key type = 'E'.
   IF sy-subrc ne 0 and
     ( not PRDISDOC is initial or
       not EHSPRPAR  is initial ).
     para_ehs = 'X' .
   ELSEIF sy-subrc eq 0 .
    MESSAGE ID msgline-id TYPE msgline-type NUMBER msgline-number
            WITH msgline-message_v1 msgline-message_v2
                 msgline-message_v3 msgline-message_v4
.

   ENDIF.
 ENDFORM.


 *----------------------------------------------------------------------*
***INCLUDE ACUSTORDERSELECTOPTIONS_STAO01 .
*----------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*&      Module  STATUS_0100  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE status_0100 OUTPUT.

  IF exit IS INITIAL.
    CALL METHOD cl_exithandler=>get_instance
      exporting
        exit_name  = 'ISU_WA_ORDER_SELECT'
        null_instance_accepted = 'X'
      CHANGING
        instance = exit.
  ENDIF.
  CALL METHOD cl_exithandler=>set_instance_for_subscreens
    EXPORTING
      instance = exit.


ENDMODULE.                 " STATUS_0100  OUTPUT
*&---------------------------------------------------------------------*
*&      Module  data_for_subscreen  OUTPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
MODULE data_for_subscreen OUTPUT.

  DATA: filter TYPE filtname.

  program = sy-repid.
  dynpro = sy-dynnr.
  filter = co_prog_order_downld.

  CALL METHOD cl_exithandler=>get_prog_and_dynp_for_subscr
    EXPORTING
      exit_name       = 'ISU_WA_ORDER_SELECT'
      calling_program = program
      calling_dynpro  = dynpro
      subscreen_area  = 'SUB'
      flt_val         = filter
    IMPORTING
      called_program  = program
      called_dynpro   = dynpro.

  CALL METHOD exit->put_data_to_screen
    EXPORTING
      flt_val = co_prog_order_downld
    CHANGING
      ranges  = customer_ranges.

ENDMODULE.                 " data_for_subscreen  OUTPUT


*----------------------------------------------------------------------*
***INCLUDE AORDER_DOWNLOAD_USER_COMMANI01 .
*----------------------------------------------------------------------*
*&---------------------------------------------------------------------*
*&      Module  user_command_0100  INPUT
*&---------------------------------------------------------------------*
*       text
*----------------------------------------------------------------------*
module user_command_0100 input.

  CALL METHOD exit->get_data_from_screen
    EXPORTING
      flt_val = co_prog_order_downld
    changing
      ranges  = customer_ranges.

endmodule.                 " user_command_0100  INPUT



method IF_EX_ISU_WA_ORDER_SELECT~GET_DATA_FROM_SCREEN.
  CLASS CL_EXIT_MASTER DEFINITION LOAD.
  DATA: EXIT_OBJ_TAB TYPE SXRT_EXIT_TAB.

  DATA: exitintf TYPE REF TO IF_EX_ISU_WA_ORDER_SELECT,
        wa_flt_cache_line TYPE REF TO sxrt_flt_cache_struct,
        flt_name TYPE FILTNAME.
  DATA: DEFAULT_CODING TYPE SXRT_BOOLEAN VALUE SXRT_TRUE.
  DATA: flt_val_db TYPE PROGRAM_ORDER_SELECT,
        desc_type(1) TYPE c, comp_num TYPE i.
  DESCRIBE FIELD flt_val_db TYPE desc_type COMPONENTS comp_num.

  FIELD-SYMBOLS:
    <flt_co1>        TYPE ANY,
    <flt_co2>        TYPE ANY.
  flt_name = flt_val.
  FIELD-SYMBOLS:
    <exit_obj>       TYPE SXRT_EXIT_TAB_STRUCT,
    <flt_cache_line> TYPE sxrt_flt_cache_struct.

  READ TABLE INSTANCE_FLT_CACHE ASSIGNING <flt_cache_line>
         WITH KEY flt_name    = flt_name
                  method_name = 'GET_DATA_FROM_SCREEN'
         .
  IF sy-subrc NE 0.

    CREATE DATA wa_flt_cache_line TYPE sxrt_flt_cache_struct.
    ASSIGN wa_flt_cache_line->* TO <flt_cache_line>.
    <flt_cache_line>-flt_name    = flt_name.
    <flt_cache_line>-method_name = 'GET_DATA_FROM_SCREEN'.

    READ TABLE INSTANCE_BADI_TABLE ASSIGNING <exit_obj>
           WITH KEY flt_val     = flt_name
                    method_name = 'GET_DATA_FROM_SCREEN'.
    If sy-subrc = 0.
      <flt_cache_line>-valid = <exit_obj>-active.
      IF <flt_cache_line>-valid = sxrt_true.
        <flt_cache_line>-obj =
             CL_EXIT_MASTER=>instantiate_imp_class(
                      CALLER       = me
                      imp_name  = <exit_obj>-imp_name
                      imp_class = <exit_obj>-imp_class ).
        MOVE <exit_obj>-imp_class to <flt_cache_line>-imp_class.
        MOVE <exit_obj>-imp_switch to <flt_cache_line>-imp_switch.
        MOVE <exit_obj>-order_num to <flt_cache_line>-order_num.
      ENDIF.
    ELSE.
      LOOP AT INSTANCE_BADI_TABLE ASSIGNING <exit_obj>
           WHERE METHOD_NAME  = 'GET_DATA_FROM_SCREEN'.
        APPEND <exit_obj> TO EXIT_OBJ_TAB.
      ENDLOOP.
      IF sy-subrc ne 0.
        CALL METHOD CL_EXIT_MASTER=>CREATE_OBJ_BY_INTERFACE_FILTER
           EXPORTING
              CALLER       = me
              INTER_NAME   = 'IF_EX_ISU_WA_ORDER_SELECT'
              METHOD_NAME  = 'GET_DATA_FROM_SCREEN'

              delayed_instance_creation    = sxrt_true
           IMPORTING
               exit_obj_tab = exit_obj_tab.

        APPEND LINES OF exit_obj_tab TO INSTANCE_BADI_TABLE.
      ENDIF.

      <flt_cache_line>-valid = sxrt_false.

      LOOP at exit_obj_tab ASSIGNING <exit_obj>
          WHERE ACTIVE   = SXRT_TRUE.

        <flt_cache_line>-valid = sxrt_true.
        flt_val_db = <exit_obj>-flt_val.
        IF comp_num = 0.
          IF flt_val NP flt_val_db.
            <flt_cache_line>-valid = sxrt_false.
          ENDIF.
        ELSE.
          DO comp_num TIMES.
            ASSIGN COMPONENT sy-index OF STRUCTURE flt_val
              TO <flt_co1>.
            ASSIGN COMPONENT sy-index OF STRUCTURE flt_val_db
              TO <flt_co2>.
            IF <flt_co1> NP <flt_co2>.
              <flt_cache_line>-valid = sxrt_false.
              EXIT.
            ENDIF.
          ENDDO.
        ENDIF.
        IF <flt_cache_line>-valid = sxrt_true.
          <flt_cache_line>-obj =
               CL_EXIT_MASTER=>instantiate_imp_class(
                        CALLER       = me
                        imp_name  = <exit_obj>-imp_name
                        imp_class = <exit_obj>-imp_class ).
          MOVE <exit_obj>-imp_class to <flt_cache_line>-imp_class.
          MOVE <exit_obj>-imp_switch to <flt_cache_line>-imp_switch.
          MOVE <exit_obj>-order_num to <flt_cache_line>-order_num.
          EXIT.
        ENDIF.
      ENDLOOP.
    ENDIF.


    INSERT <flt_cache_line> INTO TABLE INSTANCE_FLT_CACHE.


  ENDIF.


  IF <flt_cache_line>-valid = sxrt_true.
    default_coding = SXRT_FALSE.

    CALL FUNCTION 'PF_ASTAT_OPEN'
       EXPORTING
           OPENKEY = 'exxzI2{x1mRX00002aA2T0'
           TYP     = 'UE'.

    CASE <flt_cache_line>-imp_switch.
      WHEN 'VSR'.
        DATA: exc        TYPE sfbm_xcptn,                  "#EC NEEDED
              data_ref   TYPE REF TO DATA.

        IF <flt_cache_line>-eo_object is initial.
          CALL METHOD ('CL_FOBU_METHOD_EVALUATION')=>load
               EXPORTING
                  im_class_name     = <flt_cache_line>-imp_class
                  im_interface_name = 'IF_EX_ISU_WA_ORDER_SELECT'
                  im_method_name    = 'GET_DATA_FROM_SCREEN'
               RECEIVING
                  re_fobu_method    = <flt_cache_line>-eo_object
               EXCEPTIONS
                  not_found         = 1
                  OTHERS            = 2.
          IF sy-subrc = 2.
            MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
                       WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
          ENDIF.
          CHECK sy-subrc = 0.
        ENDIF.


        CLEAR data_ref.
        GET REFERENCE OF FLT_VAL INTO data_ref.
        CALL METHOD <flt_cache_line>-eo_object->set_parameter(
            im_parmname = 'FLT_VAL'
            im_value    = data_ref ).

        CLEAR data_ref.
        GET REFERENCE OF RANGES INTO data_ref.
        CALL METHOD <flt_cache_line>-eo_object->set_parameter(
            im_parmname = 'RANGES'
            im_value    = data_ref ).

        CALL METHOD <flt_cache_line>-eo_object->evaluate
             IMPORTING
                ex_exception    = exc
             EXCEPTIONS
                raise_exception = 1
                OTHERS          = 2.
        IF sy-subrc = 2.
          MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
                     WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.

        ENDIF.
      WHEN OTHERS.
        EXITINTF ?= <flt_cache_line>-OBJ.
        CALL METHOD EXITINTF->GET_DATA_FROM_SCREEN
           EXPORTING
             FLT_VAL = FLT_VAL
           CHANGING
             RANGES = RANGES.


    ENDCASE.

    CALL FUNCTION 'PF_ASTAT_CLOSE'
       EXPORTING
           OPENKEY = 'exxzI2{x1mRX00002aA2T0'
           TYP     = 'UE'.
  ENDIF.

  IF DEFAULT_CODING = SXRT_TRUE.
    IF INSTANCE_DEF_OBJ is initial.
      CREATE OBJECT INSTANCE_DEF_OBJ TYPE CL_DEF_IM_ISU_WA_ORDER_SELECT.
    ENDIF.
    exitintf ?= INSTANCE_DEF_OBJ.

    CALL FUNCTION 'PF_ASTAT_OPEN'
       EXPORTING
           OPENKEY = 'exxzI2{x1mRX00002aA2T0'
           TYP     = 'UE'.

    CALL METHOD EXITINTF->GET_DATA_FROM_SCREEN
       EXPORTING
         FLT_VAL = FLT_VAL
       CHANGING
         RANGES = RANGES.



    CALL FUNCTION 'PF_ASTAT_CLOSE'
       EXPORTING
           OPENKEY = 'exxzI2{x1mRX00002aA2T0'
           TYP     = 'UE'.
  ENDIF.
endmethod.




 
1.	Einleitung/Kundenanforderung
In der UST-S.VM.0001-003 „Vertragspositionen verwalten“ AK14 wird beschrieben, dass HWS in der Lage sein möchte, Lagepläne oder Fotos mit einer Vertragsposition zu verknüpfen. Die entsprechenden Dokumente sollen dann im Verlauf der Dispo zur Verfügung stehen und auch in das BMS übertragen werden, damit der Fahrer Zugriff darauf hat. 

2.	Standardprozesse
Am Vertrag im SAP gibt es bereits eine Standardlösung, um Dokumente hochzuladen. Da HWS das d3 als DMS anbinden möchte, muss das Hochladen von Dokumenten entsprechend konfiguriert werden. 
 
Im P&D können zu Services Dokumente angehangen und angezeigt werden. 

3.	Erweiterungen/Anpassungen
Im ersten Schritt muss eine eigene Dokumentenart ZWR_LOGDAT – „Logistikinformationen“ für die Lagepläne bzw. Anfahrtsskizzen geschaffen werden. Diese wird zum einen benötigt, um die Dokumente kategorisiert ins d3 hochzuladen, zum anderen dafür, dass klar ist, welche Dokumente tatsächlich an die Dispo übertragen werden sollen. Denn am Vertrag können beliebige Dokumente hochgeladen werden. 
Die Dokumentenart wird nur pdf-Dateien zulassen. Fotos müssen also vor dem Hochladen in eine entsprechende pdf-Datei umgewandelt werden. 
Weiterhin muss dann eine Übertragung des Dokuments ans P&D stattfinden, wenn ein Auftrag/Service zu einem solchen Vertrag angelegt wird. Es läuft im SAP-System ein Job, der regelmäßig aus den entstandenen Aufträgen aus dem IC/ der EWAORDCREA Services für das P&D macht. Das Programm aus dem Job kann auch manuell über die EWAORDERDOWN angesteuert werden, indem das Formular /PLCP/WA_ORDER_DOWNLOAD genutzt wird. Diese Erzeugung von Services sollte angepasst werden. 
Hier sollte neben der klassischen Serviceanlage ebenfalls der Vertrag geprüft werden. Wenn am Vertrag ein Dokument/ Dokumente der Dokumentenart Z_LOG_DATA hinterlegt sind, müssen diese Dokumente aus dem d3 abgerufen werden (liegen nicht im SAP selbst) und im P&D an den gerade erzeugten Service angehangen werden. 
So sollen die Disponenten in der Lage sein, die Lagepläne, Anfahrtsskizzen oder ähnliches im P&D aufzurufen. 
Es muss geprüft werden, ob das BMS-Anhänge anbietet und ob die BMS-Schnittstelle die Anhänge somit auch dem Fahrer zur Verfügung stellen kann. Für die Fälle, in denen ohne BMS gefahren wird, kann der Disponent die Dokumente öffnen und drucken. (nicht Bestandteil der Umsetzung)
Im P&D liegen die Dokumente vorerst direkt gespeichert. Auf lange Sicht soll ein Löschreport die Dokumente veralteter Services löschen. (nicht Bestandteil der Umsetzung). 

4.	Testszenarien/Akzeptanzkriterien
#	Szenario	Erwartetes Ergebnis
1.	Übers IC einen Vertrag anlegen oder einen vorhandenen Vertrag benutzen und einen Lageplan hochladen über die Funktion „Business Document ablegen“ 	Es gibt eine Option die Datei unter „Logistikinformationen“ abzulegen. Der Lageplan muss im Format pdf vorliegen. Das Dokument wird gesichert im d3 und kann dort auch eingesehen werden. (nicht prüfbar durch Prologa) Der Vertrag kann gesichert werden. 
2.	Aufträge zu dem Vertrag anlegen	Im P&D ist an den Services, die aus den Aufträgen entstanden sind, die Dokumente hinterlegt und einsehbar. 
		

5.	Alternativen
-	Übertragung der Informationen per Mail oder Ausdruck. 
6.	Aufwand
???



  INCLUDE aorder_download_user_commani01.
