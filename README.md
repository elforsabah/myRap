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
  INCLUDE aorder_download_status_0100o01.

  INCLUDE aorder_download_user_commani01.
