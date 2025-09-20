managed implementation in class zbp_i_wr_weighingsession unique;
strict( 2 );

with draft;

define behavior for ZI_WR_WEIGHINGSESSION alias WeighingSession
persistent table zwr_weighsession
draft table ZWR_WEIGHB_DD
lock master
total etag Grossweight
authorization master ( instance )
etag master Grossweight

{
//  Keys must be readonly in strict draft
  field (readonly) Sessionid;  // <-- use your exact CDS element names
  field (numbering: managed) Sessionid;
create;
update;
delete;

draft action Edit;
draft action Activate;
draft action Discard;
draft action Resume;                 // <-- required
draft determine action Prepare;      //<-- required

action NextStep result [1] $self; // server validates & increments Step
action Submit result [1] $self; // final checks; printing trigger optional

action identifyCard parameter ZAE_WR_WEIGHINGSESSION result [1] $self ;
action determineWeight parameter ZAE_WR_WEIGHB_DW  result [1] $self;
function printSlip parameter ZAE_WR_WEIGHB_PRINT  result [1] $self;


//action identifyCard  result [1] $self;

validation validateStep1 on save { field Vbeln, Sessionid; } // Identification
validation validateStep2 on save { field Loadtype; } // Load type
validation ValidateLoadType on save { field LoadType; } // Calls a method to check against VH

determination calcNet on modify { field Grossweight, Tareweight; } // Weighing math
}


@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Weighing Session'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHINGSESSION
  as select from zwr_weighsession
  association [0..*] to ZI_WR_SALESITEM_CONTRACTVH as _SalesItems
    on _SalesItems.SalesOrder = $projection.Vbeln
 
 
{
      key sessionid as Sessionid,
      vbeln as Vbeln,
      @UI.textArrangement: #TEXT_LAST
      loadtype as LoadType,
      @Semantics.quantity.unitOfMeasure: 'Grossweightunit'
      grossweight as Grossweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      grossweightunit as Grossweightunit,
      @Semantics.quantity.unitOfMeasure: 'Tareweightunit'
      tareweight as Tareweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      tareweightunit as Tareweightunit,
      @Semantics.quantity.unitOfMeasure: 'Netweightunit'
     
      netweight as Netweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      pdfbase64 as PDFBASE64,
      netweightunit as Netweightunit,
      step as Step,
      step1ok as Step1Ok,
      step2ok as Step2Ok,
      issummited as IsSummited,
     
      _SalesItems
}

@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Materials of Contract'
@ObjectModel.dataCategory: #VALUE_HELP
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
@ObjectModel.resultSet.sizeCategory: #XS  // Make it a Drop Down
define view entity ZI_WR_SALESITEM_CONTRACTVH 
    with parameters
    P_SalesOrder : vbeln_va
  as select distinct from I_SalesDocumentItem as salesitem  
   inner join I_MaterialText as _text on salesitem.Material = _text.Material
                                      and _text.Language = $session.system_language
  association to /watp/tavvmara as _tavvmara on _text.Material = _tavvmara.matnr
{
//    @UI.hidden: true
    key salesitem.SalesDocument as SalesOrder,
//    @UI.hidden: true
    key salesitem.SalesDocumentItem as SalesOrderitem,
    _text.Material as Material,
    _text.MaterialName as MaterialText,
    _tavvmara.avvcode as Avvcode,
//    @UI.hidden: true
    _text.Language as Language  ,    
    
    _tavvmara
} where salesitem.SalesDocument = $parameters.P_SalesOrder

@EndUserText.label: 'Abstract View for Identification'
define abstract entity ZAE_WR_WEIGHINGSESSION
  
{
    Vbeln : vbeln_va;
//    loadtype : load_type null; 
    
}



@EndUserText.label: 'Print Slip Result'
define abstract entity ZAE_WR_WEIGHB_PRINT_Result
  
{
    pdfbase64 : abap.rawstring(0);
    
}

@EndUserText.label: 'Print Slip'
define abstract entity ZAE_WR_WEIGHB_PRINT
  
{
     Vbeln : vbeln_va;
     Loadtype : matnr;
     Weight : abap.char(60);
     WeighUnit: abap.char(3);
}
CLASS lhc_WeighingSession DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR WeighingSession RESULT result.

    METHODS identifyCard FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~identifyCard RESULT result.

    METHODS NextStep FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~NextStep RESULT result.

    METHODS setloadType FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~setloadType RESULT result.

    METHODS Submit FOR MODIFY
      IMPORTING keys FOR ACTION WeighingSession~Submit RESULT result.

ENDCLASS.

CLASS lhc_WeighingSession IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD identifyCard.
    READ ENTITIES OF zi_wr_weighbridge IN LOCAL MODE
    ENTITY WeighingSession
    ALL FIELDS WITH CORRESPONDING #( keys )
    RESULT DATA(lt_sessions).

  LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_session>).

    SELECT SINGLE vbeln FROM vbak
      WHERE vbeln = @<ls_session>-%param-vbeln
      INTO @DATA(lv_vbeln).

    IF sy-subrc = 0.
*      " ✓ Valid: mark step ok
*      MODIFY ENTITIES OF zi_wr_weighbridge IN LOCAL MODE
*        ENTITY WeighingSession
*        UPDATE FIELDS ( step1ok )
*        WITH VALUE #( ( %tky = <ls_session>-%tky  step1ok = abap_true ) ).

      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %msg = new_message(
                 id       = 'ZWR_WEIGHBRIGE_MESS'
                 number   = '000'
                 severity = if_abap_behv_message=>severity-success
                 v1       = 'Contract is valid' ) )
        TO reported-weighingsession.
      result = VALUE #( ( %tky = <ls_session>-%tky ) ).
    ELSE.
*      " ✗ Invalid: keep step NOT ok and fail the action
*      MODIFY ENTITIES OF zi_wr_weighingsession IN LOCAL MODE
*        ENTITY WeighingSession
*        UPDATE FIELDS ( step1ok )
*        WITH VALUE #( ( %tky = <ls_session>-%tky  step1ok = abap_false ) ).

*          APPEND VALUE #(
*          %tky       = <ls_session>-%tky
*          %msg       = new_message(
*                         id       = 'ZWR_WEIGHBRIGE_MESS'
*                         number   = '000'
*                         severity = if_abap_behv_message=>severity-error
*                         v1       = <ls_session>-%param-vbeln )
**              %element-vbeln = if_abap_behv=>mk-on                                 " <-- bind message to Vbeln
*            ) TO reported-weighingsession.
*
*
*                " Fail the action to reject the promise
*          APPEND VALUE #(
*            %tky                 = <ls_session>-%tky
*            %action-identifycard = if_abap_behv=>mk-on
*            %fail-cause          = if_abap_behv=>cause-readonly
*          ) TO failed-weighingsession.


" Append unbound error message to %other (no %tky or %element)
      APPEND VALUE #(
        %tky       = <ls_session>-%tky
        %msg = new_message(
                       id = 'ZWR_WEIGHBRIGE_MESS'
                       number = '001'  " Adjust to your error message number
                       severity = if_abap_behv_message=>severity-error
                       v1 = <ls_session>-%param-vbeln )
      ) TO reported-weighingsession.

      " Fail the action to reject the promise
      APPEND VALUE #(
        %tky = <ls_session>-%tky
        %action-identifycard = if_abap_behv=>mk-on
        %fail-cause = if_abap_behv=>cause-readonly  " Or use cause-unspecific if more appropriate
      ) TO failed-weighingsession.


   ENDIF.
  ENDLOOP.


  ENDMETHOD.

  METHOD NextStep.
  ENDMETHOD.

  METHOD setloadType.
  ENDMETHOD.

  METHOD Submit.
  ENDMETHOD.

ENDCLASS.

class ZCL_WR_WEIGHBRIDGE_HELPER definition
  public
  create public .

public section.

  class-methods CL_GET_CONTRACT
    importing
      !IV_CARDID type STRING
    returning
      value(RV_CONTRACT) type VBELN_VA .
  class-methods CL_GET_OPEN_WEIGHPROC
    importing
      !IV_CONTRACT type VBELN_VA
    returning
      value(RV_WEIGHINGNR) type EWAWA_WEIGHINGNR .
  class-methods CL_HAS_OPEN_WEIGHINGPROC
    importing
      !IV_CARDID type STRING optional
      !IV_CONTRACTID type VBELN_VA optional
    returning
      value(RV_HAS_WPROC) type ABAP_BOOL .
  class-methods CL_WEIGHING
    importing
      !IV_CONTRACTID type VBELN_VA
      !IV_WASTEID type WASTE_TYPE optional
      !IV_LICENCE_PLATE type LICENSE_NUM optional
      !IV_SIMULATION type ABAP_BOOL default ABAP_FALSE
      !IV_USER type XUBNAME default SY-UNAME
      !IV_COMMIT type ABAP_BOOL default ABAP_TRUE
    exporting
      !IV_WEIGHING_RESULT type ZWR_SWAA_SELFWEIGH_RESULT
    raising
      CX_EEWA_BASE .
  class-methods FORM_TO_BASE64
    importing
      !IV_FORM type CHAR30 default 'IS_U_WA_SF_WEIGHINGPROCESS'          " SFP form name
      !IV_LANG type SYLANGU default SY-LANGU
      !IV_SHOW type ABAP_BOOL default ABAP_TRUE          " Show Base64 preview
      !IV_PDF type ABAP_BOOL default ABAP_FALSE           " Save PDF file
      !IV_B64 type ABAP_BOOL default ABAP_FALSE           " Save Base64 text
      !IV_PDFPTH type STRING optional "DEFAULT 'C:\Temp\adobe_form.pdf'
      !IV_B64PTH type STRING optional
      !IV_PREVLN type I default 200        " Preview chars
      !IV_WA type EWA_WA_WEIGHPROC
    returning
      value(R_BASE64) type XSTRING . " Return the Base64 string
protected section.
private section.
ENDCLASS.



CLASS ZCL_WR_WEIGHBRIDGE_HELPER IMPLEMENTATION.


  METHOD form_to_base64.
    DATA: ls_outparams          TYPE sfpoutputparams,
          ls_docparams          TYPE sfpdocparams,
          lv_fm_name            TYPE rs38l_fnam,
          ls_formoutput         TYPE fpformoutput,
          lv_pdf_xstr           TYPE xstring,
          lv_pdf_b64            TYPE string,
          lv_job_opened         TYPE abap_bool,
          ls_ARCHIVE_INDEX      TYPE  toa_dara,
          ls_ARCHIVE_INDEX_TAB  TYPE  tsfdara,
          ls_ARCHIVE_PARAMETERS TYPE  arc_params,
          ls_CONTROL_PARAMETERS TYPE  ssfctrlop,
          ls_MAIL_APPL_OBJ      TYPE  swotobjid,
          ls_MAIL_RECIPIENT     TYPE  swotobjid,
          ls_MAIL_SENDER        TYPE  swotobjid.

    ls_outparams-nodialog = abap_true.
    ls_outparams-getpdf = abap_true.
    ls_docparams-langu = iv_lang.

    " Fill application data for the form interface
    " Example values - adjust based on your form's requirements

    TRY.
        " Open FP job
        CALL FUNCTION 'FP_JOB_OPEN'
          CHANGING
            ie_outputparams = ls_outparams.
            lv_job_opened = abap_true.

        " Resolve generated FM for form name
        CALL FUNCTION 'FP_FUNCTION_MODULE_NAME'
          EXPORTING
            i_name     = iv_form
          IMPORTING
            e_funcname = lv_fm_name.

        " === If your SFP interface needs additional application data, fill it here ===
        " ================================================================

        " Call the form FM
        CALL FUNCTION lv_fm_name
          EXPORTING
            /1bcdwb/docparams  = ls_docparams
            archive_index      = ls_archive_index
            archive_index_tab  = ls_archive_index_tab
            archive_parameters = ls_archive_parameters
            control_parameters = ls_control_parameters
            mail_appl_obj      = ls_mail_appl_obj
            mail_recipient     = ls_mail_recipient
            mail_sender        = ls_mail_sender
            wa                 = iv_wa
          IMPORTING
            /1bcdwb/formoutput = ls_formoutput
          EXCEPTIONS
            usage_error        = 1
            system_error       = 2
                                 internal_error= 3
            OTHERS             = 4.
        IF sy-subrc <> 0.
          MESSAGE ID sy-msgid TYPE sy-msgty NUMBER sy-msgno
                  WITH sy-msgv1 sy-msgv2 sy-msgv3 sy-msgv4.
        ENDIF.

      CLEANUP.
        IF lv_job_opened = abap_true.
          " Always close job
          CALL FUNCTION 'FP_JOB_CLOSE'
            EXCEPTIONS
              OTHERS = 1.
        ENDIF.
    ENDTRY.

    " Get PDF as XSTRING (newer) or convert from SOLIX (older)
    IF ls_formoutput-pdf IS NOT INITIAL.
      lv_pdf_xstr = ls_formoutput-pdf.
    ELSE.
      MESSAGE 'No PDF returned by ADS.' TYPE 'E'.
    ENDIF.

    " XSTRING -> Base64
    lv_pdf_b64 = cl_http_utility=>if_http_utility~encode_x_base64( lv_pdf_xstr ).

    " Optional: display a short preview of Base64
    IF iv_show = abap_true.
      DATA(lv_len) = iv_prevln.
      IF lv_len <= 0 OR lv_len > strlen( lv_pdf_b64 ).
        lv_len = strlen( lv_pdf_b64 ).
      ENDIF.
      WRITE: / 'Base64 (preview):'.
      ULINE.
      WRITE: / lv_len.
      SKIP.
      WRITE: / |Total Base64 length: { strlen( lv_pdf_b64 ) } chars|.
    ENDIF.

    " Optional: save PDF to frontend
    IF iv_pdf = abap_true.
      DATA(lt_solix) = cl_bcs_convert=>xstring_to_solix( lv_pdf_xstr ).
      cl_gui_frontend_services=>gui_download(
        EXPORTING
          filename = iv_pdfpth
          filetype = 'BIN'
        CHANGING
          data_tab = lt_solix
        EXCEPTIONS
          OTHERS = 1 ).
      IF sy-subrc = 0.
        WRITE: / |PDF saved to: { iv_pdfpth }|.
      ELSE.
        WRITE: / 'Could not save PDF to frontend.' COLOR COL_NEGATIVE.
      ENDIF.
    ENDIF.

    " Optional: save Base64 text to frontend
    IF iv_b64 = abap_true.
      DATA(lt_lines) = VALUE stringtab( ( lv_pdf_b64 ) ).
      cl_gui_frontend_services=>gui_download(
        EXPORTING
          filename = iv_b64pth
          filetype = 'ASC'
        CHANGING
          data_tab = lt_lines
        EXCEPTIONS
          OTHERS = 1 ).
      IF sy-subrc = 0.
        WRITE: / |Base64 saved to: { iv_b64pth }|.
      ELSE.
        WRITE: / 'Could not save Base64 to frontend.' COLOR COL_NEGATIVE.
      ENDIF.
    ENDIF.

    IF iv_pdf = abap_false AND iv_b64 = abap_false AND iv_show = abap_false.
      WRITE: / 'Done. (Nothing selected to show or save.)'.
    ENDIF.

    r_base64 = lv_pdf_xstr. " Return the Base64 string

  ENDMETHOD.


  method CL_GET_CONTRACT.
    RV_CONTRACT = IV_CARDID. "design definition.
  endmethod.


  method CL_HAS_OPEN_WEIGHINGPROC.
    data:
      LV_CONTRACT     type VBELN_VA.

    RV_HAS_WPROC = ABAP_FALSE.
    LV_CONTRACT = IV_CONTRACTID.
    if IV_CARDID is not initial and LV_CONTRACT is initial.
      LV_CONTRACT = CL_GET_CONTRACT(
        exporting
          IV_CARDID   = IV_CARDID
      ).
    endif.

    if LV_CONTRACT is not initial and
       CL_GET_OPEN_WEIGHPROC( exporting IV_CONTRACT   = LV_CONTRACT ) is not initial.
      RV_HAS_WPROC = ABAP_TRUE.
    endif.
  endmethod.


  method CL_WEIGHING.
    data:
      LS_PROCESS        type ZCL_WR_WAA_TA_SELFWEIGHING=>STY_WEIGHING_REQUEST,
      LS_PROCESS_RESULT type ZWR_SWAA_SELFWEIGH_RESULT.

    clear:
      IV_WEIGHING_RESULT.

    select single *
      into @data(LS_HW_PROFILE)
      from EEWA_SCALE_USER
      where XUSER = @IV_USER.

    if SY-SUBRC is initial.
      LS_PROCESS-WEIGHINGID = CL_GET_OPEN_WEIGHPROC(
        exporting
          IV_CONTRACT   = IV_CONTRACTID
      ).

      LS_PROCESS-DEVICEGROUP = LS_HW_PROFILE-DEVICEGROUPNR.
      LS_PROCESS-FACILITYID  = LS_HW_PROFILE-WDPLANT.
      LS_PROCESS-PROCESSTYPE = CL_EEWA_BO_WDORDERWEIGH=>CPROCESSTYPE_INBOUND.
      LS_PROCESS-SIMULATION  = IV_SIMULATION.
      LS_PROCESS-WASTEID     = IV_WASTEID.

      TA_BEGIN_NAMED_VAR TA ZCL_WR_WAA_TA_SELFWEIGHING=>C_TA_NAME.
        TA_DO_RAISE_EXCEPTION TA.
        if IV_COMMIT = ABAP_TRUE.
          TA_DO_COMMIT TA.
        else.
          TA_NO_COMMIT TA.
        endif.
        TA_SET_PARAM TA GS_PROCESS_DATA LS_PROCESS.
        TA_EXECUTE TA ZZ_BOOK_WEIGHING.
        TA_GET_PARAM TA GS_RESULT_DATA LS_PROCESS_RESULT.
      TA_END TA.

    endif.

  endmethod.


  method CL_GET_OPEN_WEIGHPROC.
    types:
      begin of TY_WEIGHINGLEAD,
        WEIGHINGNR_LEAD type EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD,
      end of TY_WEIGHINGLEAD.
    data:
      LV_CONTRACT     type VBELN_VA,
      LV_GROSSID      type EWA_WA_WEIGHPROC-GROSS_ID,
      LV_TAREID       type EWA_WA_WEIGHPROC-TARE_ID,
      LV_WEIGHINGLEAD type EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD,
      LV_MAX          type INT4 value 1,
      LT_WEIGHINGNR   type standard table of TY_WEIGHINGLEAD.

    clear RV_WEIGHINGNR.
    if IV_CONTRACT is not initial.
      DATA_BO_RANGE X.
      SET_BO_RANGE X CONTRACT_VBELN LV_CONTRACT VBELN_VA.
      SET_BO_RANGE_CMPLT X GROSS_ID LV_GROSSID EWA_WA_WEIGHPROC-GROSS_ID I NE.
      SET_BO_RANGE_CMPLT X TAREID LV_TAREID EWA_WA_WEIGHPROC-TARE_ID I EQ.
      SET_BO_RANGE_CMPLT X WEIGHINGNR_LEAD LV_WEIGHINGLEAD EWA_WA_WEIGHPROC-WEIGHINGNR_LEAD I NE.

      CL_EEWA_OR_WEIGHINGPROCESS=>CL_READ(
        exporting
          PAR_OBJTYPE = CL_EEWA_BO_WEIGHINGPROCESS=>COT_WEIGHINGPROCESS
          PAR_DOMAIN  = 'OPEN'
          PAR_RANGES  = LRANGEXS
          PAR_MAXROWS = LV_MAX
        importing
          PAR_TABLE   = LT_WEIGHINGNR
      ).
      if LINES( LT_WEIGHINGNR ) > 0.
        RV_WEIGHINGNR = LT_WEIGHINGNR[ 1 ]-WEIGHINGNR_LEAD.
      endif.
    endif.
  endmethod.
ENDCLASS.
