<img width="1896" height="871" alt="image" src="https://github.com/user-attachments/assets/64773308-3edb-4de3-9c1e-cf36bdd18832" />



@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Root for LANF'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_LANF_ROOT as select from ddddlsrc
{  
    key cast('' as abap.char(30)) as Techkey
} where 1 = 2 // Ensures no data is returned


@EndUserText.label: 'Message Structure'
define abstract entity ZI_MESSAGE
  
{
   key TechKey       : abap.char(1);
  Msgid             : msgid;
  Msgno             : msgnr;
  Msgty             : msgty;
  Msgv1             : symsgv;
  Msgv2             : symsgv;
  Msgv3             : symsgv;
  Msgv4             : symsgv;
  
//  _Parent           : association to parent ZI_LANF_RESPONSE on $projection.TechKey = _Parent.TechKey;
}



@EndUserText.label: 'Input for Document Attachment'
define root abstract entity ZI_ATTACH_INPUT

{
//  key TechKey       : abap.char(1);
  key Vbeln             : vbeln_va;     // LANF number
  Title             : text100;      // Title/Description
  Url               : url;          // Document URL (for archive link)
    
}


@EndUserText.label: 'Input for LANF Creation'
define root abstract entity ZI_LANF_CREATE_INPUT
  
{
//  key TechKey     : abap.char(1); // Required for root abstract entity
  key ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  DeliveryDate      : kbdat;        // Delivery/Service date (FBUDA/KETDAT)
  CustomerRef       : bstkd;        // Customer reference (BSTKD)
  Description       : abap.char(225);      // Description (VBAK-TEXT)
  _Positions        : composition [0..*] of ZI_LANF_POSITION_INPUT;
    
}
@EndUserText.label: 'Response Structure'
define root abstract entity ZI_LANF_RESPONSE
{
 key Vbeln             : vbeln_va;     // Created LANF number
}
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Root for LANF'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_LANF_ROOT as select from ddddlsrc
{  
    key cast('' as abap.char(30)) as Techkey
} where 1 = 2 // Ensures no data is returned


abstract;
strict ( 2 );

define behavior for ZI_ATTACH_INPUT //alias <alias_name>
{

}

abstract;
strict ( 1 );
with hierarchy;
define behavior for ZI_LANF_CREATE_INPUT alias createin
{
 association _Positions;
}

define behavior for ZI_LANF_POSITION_INPUT alias posIn
{

association _Parent;
}

abstract;
strict ( 2 );
define behavior for ZI_LANF_RESPONSE
{
}


unmanaged implementation in class zbp_i_lanf_root unique;

define behavior for ZI_LANF_ROOT alias lanfroot
//persistent table ddddlsrc
lock master
authorization master ( instance )
//etag master <field_name>
{

// // First endpoint: Create LANF
  static action CreateLanf deep parameter ZI_LANF_CREATE_INPUT  result [1] ZI_LANF_RESPONSE;
//
//  // Second endpoint: Attach PDF/Document Link
  static action AttachDocument  parameter ZI_ATTACH_INPUT result [1] ZI_LANF_RESPONSE;



//  create;
//  update;
//  delete;
}

CLASS lhc_lanfroot DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    TYPES tt_lanf_position_input TYPE STANDARD TABLE OF zi_lanf_position_input WITH DEFAULT KEY.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR lanfroot RESULT result.

    METHODS read FOR READ
      IMPORTING keys FOR READ lanfroot RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK lanfroot.

    METHODS attachdocument FOR MODIFY
      IMPORTING keys FOR ACTION  lanfroot~attachdocument RESULT result.

    METHODS createlanf FOR MODIFY
      IMPORTING keys FOR ACTION lanfroot~createlanf RESULT result.

    METHODS map_positions_to_contract
      IMPORTING iv_contract_vbeln TYPE vbeln_va
                it_positions      TYPE tt_lanf_position_input
      CHANGING  ct_order_items    TYPE  bapisditm_tt
                ct_order_itemsx   TYPE  bapisditmx_tt
                ct_return         TYPE bapiret2_tab.


ENDCLASS.

CLASS lhc_lanfroot IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.




METHOD createlanf.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  DATA(ls_input) = ls_key-%param.

  DATA lv_contract TYPE vbeln_va.
  lv_contract = |{ ls_input-contractvbeln ALPHA = IN }|.

  DATA lt_pos_rfc  TYPE zlanf_pos_in_tt.
  lt_pos_rfc = VALUE #(
    FOR p IN ls_input-_positions
    WHERE ( menge > 0 )
    ( matnr = p-matnr
      menge = p-menge
      meins = p-meins )
  ).

  DATA lt_return  TYPE bapiret2_tab.
  DATA lv_new_so  TYPE vbeln_va.
  DATA lv_sysmsg  TYPE char60.
  DATA lv_commmsg TYPE char60.

  CALL FUNCTION 'Z_LANF_CREATE_DMREQ_RFC'
    DESTINATION 'NONE'
    EXPORTING
      iv_contract     = lv_contract
      iv_deliverydat  = ls_input-deliverydate
      iv_customerref  = ls_input-customerref
      iv_auart        = 'ZLRA'
    IMPORTING
      ev_vbeln        = lv_new_so
    TABLES
      it_positions    = lt_pos_rfc
      et_return       = lt_return
    EXCEPTIONS
      system_failure        = 1 MESSAGE lv_sysmsg
      communication_failure = 2 MESSAGE lv_commmsg
      OTHERS                = 3.

  IF sy-subrc <> 0 OR lv_new_so IS INITIAL
     OR line_exists( lt_return[ type = 'E' ] )
     OR line_exists( lt_return[ type = 'A' ] ).

    "you can still report errors via reported-... even if response is VBELN-only
    LOOP AT lt_return INTO DATA(ls_r) WHERE type CA 'EA'.
      APPEND VALUE #( %msg = new_message(
        id       = ls_r-id
        number   = ls_r-number
        v1       = ls_r-message_v1
        v2       = ls_r-message_v2
        v3       = ls_r-message_v3
        v4       = ls_r-message_v4
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDLOOP.

    IF sy-subrc <> 0.
      APPEND VALUE #( %msg = new_message(
        id       = '00' number = '001'
        v1       = |RFC failed: { COND string(
                   WHEN sy-subrc = 1 THEN lv_sysmsg
                   WHEN sy-subrc = 2 THEN lv_commmsg
                   ELSE |SUBRC { sy-subrc }| ) }|
        severity = if_abap_behv_message=>severity-error ) ) TO reported-lanfroot.
    ENDIF.

    failed-lanfroot = VALUE #( ( %cid = ls_key-%cid ) ).
    RETURN.
  ENDIF.

  "Return only VBELN
  result = VALUE #(
    ( %cid   = ls_key-%cid
      %param = VALUE #( vbeln = |{ lv_new_so ALPHA = OUT }| ) )
  ).

ENDMETHOD.


METHOD attachdocument.

  DATA: ls_input         TYPE zi_attach_input,
        lt_return_attach TYPE bapiret2_tab,
        lv_vbtyp         TYPE vbak-vbtyp,
        lv_busobj        TYPE swo_objtyp.

  READ TABLE keys INTO DATA(ls_key) INDEX 1.
  ls_input = CORRESPONDING #( ls_key-%param ).

  DATA(lv_vbeln) = |{ ls_input-vbeln ALPHA = IN }|.

  " Determine SD doc category (VBTYP)
  SELECT SINGLE vbtyp FROM vbak
    WHERE vbeln = @lv_vbeln
    INTO @lv_vbtyp.

  IF sy-subrc <> 0.
    APPEND VALUE #( type = 'E' id = '00' number = '001'
      message = |Sales document { lv_vbeln } not found| ) TO lt_return_attach.
  ELSE.
    " Determine correct BOR object for that category (avoids BUS2032/L issue)
    CALL FUNCTION 'SD_OBJECT_TYPE_DETERMINE'
      EXPORTING
        i_document_type   = lv_vbtyp
      IMPORTING
        e_business_object = lv_busobj.


    " Attach URL as GOS/BDS link
    CALL FUNCTION 'BDS_BUSINESSDOCUMENT_CRE_O_URL'
      EXPORTING
        classname       = lv_busobj
        classtype       = 'BO'
        object_key      = lv_vbeln
        url             = ls_input-url
        url_description = ls_input-title
      EXCEPTIONS
        nothing_found   = 1
        parameter_error = 2
        not_allowed     = 3
        error_kpro      = 4
        internal_error  = 5
        OTHERS          = 6.

    IF sy-subrc <> 0.
      APPEND VALUE #( type = 'E' id = sy-msgid number = sy-msgno
        message_v1 = sy-msgv1 message_v2 = sy-msgv2 message_v3 = sy-msgv3 message_v4 = sy-msgv4
        message = |Attach URL failed (SY-SUBRC={ sy-subrc })| ) TO lt_return_attach.
    ELSE.
      APPEND VALUE #( type = 'S' id = '00' number = '000'
        message = |URL attached successfully| ) TO lt_return_attach.
    ENDIF.
  ENDIF.

  " Return VBELN + messages
  result = VALUE #(
    ( %param = VALUE #(
*        techkey   = 'X'
        vbeln     = lv_vbeln
*        _messages = VALUE #(
*          FOR r IN lt_return_attach (
*            techkey = 'X'
*            msgid   = r-id
*            msgno   = r-number
*            msgty   = r-type
*            msgv1   = r-message_v1
*            msgv2   = r-message_v2
*            msgv3   = r-message_v3
*            msgv4   = r-message_v4 ) )

            ) ) ).

ENDMETHOD.

METHOD map_positions_to_contract.

  SELECT posnr, matnr
    FROM vbap
    WHERE vbeln = @iv_contract_vbeln
    INTO TABLE @DATA(lt_contract_pos).

  LOOP AT it_positions ASSIGNING FIELD-SYMBOL(<pos>) WHERE menge > 0.

    DATA(lv_matnr) = <pos>-matnr.
    CALL FUNCTION 'CONVERSION_EXIT_MATN1_INPUT'
      EXPORTING input  = lv_matnr
      IMPORTING output = lv_matnr.

    READ TABLE lt_contract_pos ASSIGNING FIELD-SYMBOL(<contr_pos>)
      WITH KEY matnr = lv_matnr.

    IF sy-subrc = 0.
      APPEND VALUE bapisditm(
        itm_number = <contr_pos>-posnr
        material   = lv_matnr
        target_qty = <pos>-menge
        target_qu  = <pos>-meins
        ref_doc    = iv_contract_vbeln
        ref_doc_it = <contr_pos>-posnr
        ref_doc_ca = 'G' ) TO ct_order_items.

      APPEND VALUE bapisditmx(
        itm_number = <contr_pos>-posnr
        updateflag = 'I'
        material   = 'X'
        target_qty = 'X'
        target_qu  = 'X'
        ref_doc    = 'X'
        ref_doc_it = 'X'
        ref_doc_ca = 'X' ) TO ct_order_itemsx.
    ELSE.
      APPEND VALUE bapiret2(
        type   = 'E'
        id     = '00'
        number = '001'
        message = |No position for MATNR { lv_matnr } in contract { iv_contract_vbeln }| ) TO ct_return.
    ENDIF.

  ENDLOOP.

ENDMETHOD.



ENDCLASS.

CLASS lsc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_saver.
  PROTECTED SECTION.

    METHODS finalize REDEFINITION.

    METHODS check_before_save REDEFINITION.

    METHODS save REDEFINITION.

    METHODS cleanup REDEFINITION.

    METHODS cleanup_finalize REDEFINITION.

ENDCLASS.

CLASS lsc_zi_lanf_root IMPLEMENTATION.

  METHOD finalize.
  ENDMETHOD.

  METHOD check_before_save.
  ENDMETHOD.

  METHOD save.
  ENDMETHOD.

  METHOD cleanup.
  ENDMETHOD.

  METHOD cleanup_finalize.
  ENDMETHOD.

ENDCLASS.



Wednesday, July 8, 2026, 6:59:34 AM CEST

Frontend Error: Unspecified error occurred. See Error Context for more details
Gateway-Fehlerprotokolleinträge für meinen Benutzer (HWSB10035) | HWSB10035 (Elvis Mbah Forsab)

Content
Header Information
Service Information
Error Context
Source Code Extract
Active Calls/Events
Header Information
  Type	Frontend Error
  Short Text	Unspecified error occurred. See Error Context for more details
  Transaction ID	F14022DB810C0030E006A0EE5DF6F67C (Replay in GW Client)
  Package	/IWCOR/SODSTD_PROC
  Application Component	BC-ESI-ESF-GW
  Date/Time	08.07.2026 06:59:34 (System)
  Username	HWSB10035 (Elvis Mbah Forsab)
  Client	440
  Request Kind	Server OData V4
Service Information
  Service Namespace	
  Service Name	ZSB_LANF_BMS_INTERFACE_V4
  Service Version	0001
  Group ID	ZSB_LANF_BMS_INTERFACE_V4
  Service Repository	SRVD_A2X
  Destination	NONE
Error Context
 –ERROR_CONTEXT	
      ERROR_INFO	Unspecified error occurred. See Error Context for more details
  –ERROR_RESOLUTION	
       SAP_NOTE	See SAP Note 1797736 for error analysis
       LINK_TO_SAP_NOTE	https://service.sap.com/sap/support/notes/1797736
  –Exceptions	
   –/IWCOR/CX_OD_UNSUPP_MEDIA_TYPE	
        Text	The server is refusing to process the request because the entity has an unsupported format
        Raise Location	/IWCOR/CL_OD_PROC_DISPATCHER
  –SERVICE_INFO	
       GROUP_ID	ZSB_LANF_BMS_INTERFACE_V4
       SERVICE_REPOSITORY	SRVD_A2X
       SERVICE_NAME	ZSB_LANF_BMS_INTERFACE_V4
       VERSION	0001
       SYSTEM_ALIAS	LOCAL
       DESTINATION	NONE
  –SYSTEM_INFO	
       REQUEST_ID	005056B8C72B1FD19ECF217CBF498000
       REQUEST_URI	/sap/opu/odata4/sap/zsb_lanf_bms_interface_v4/srvd_a2x/sap/zsb_lanf_bms_interface_v4/0001/ZI_LANF_ROOT/com.sap.gateway.srvd_a2x.zsb_lanf_bms_interface_v4.v0001.CreateLanf
       REMOTE_ADDRESS	127.0.0.1
       APPLICATION_SERVER	evhsap-srv08_RI4_01
       HUB_VERSION_INFO	RI4/440, Rel. 7.58, GWHUB Version 031
       BEP_VERSION_INFO	RI4/440, Rel. 7.58, GWBEP Version 031
Source Code Extract
25
26
27
28
29
30
31
32
33
34
35
36
37
38
>>
40
41
42
      TRY.
          " Parse content type.
          /iwcor/cl_rest_http_utils=>parse_content_type( EXPORTING iv_media_range = lv_content_type
                                                         IMPORTING ev_type = lv_type
                                                                   ev_subtype = lv_subtype
                                                                   ev_parameter = lv_parameter ).
        CATCH /iwcor/cx_rest_parser_error INTO lx_rest_parser_exception. " REST negotiation error
          RAISE EXCEPTION NEW /iwcor/cx_od_unsupp_media_type( previous = lx_rest_parser_exception ).
      ENDTRY.
      READ TABLE it_supported_content_type WITH KEY type = lv_type
                                                    subtype = lv_subtype
                                                    parameter = lv_parameter
           TRANSPORTING NO FIELDS.
      IF sy-subrc <> 0.
        RAISE EXCEPTION NEW /iwcor/cx_od_unsupp_media_type( ).
      ENDIF.
    ENDIF.
  ENDMETHOD.
Active Calls/Events
No.	Event	Program	Include	Line
12	METHOD	/IWBEP/CL_V4_V2_SUPPORT_FACADECP	/IWBEP/CL_V4_V2_SUPPORT_FACADECM00Q	22
11	METHOD	/IWBEP/CL_V4_LOGGER===========CP	/IWBEP/CL_V4_LOGGER===========CM001	3
10	METHOD	/IWBEP/CL_OD_ERROR_HANDLER====CP	/IWBEP/CL_OD_ERROR_HANDLER====CM006	325
9	METHOD	/IWBEP/CL_OD_ROOT_HANDLER=====CP	/IWBEP/CL_OD_ROOT_HANDLER=====CM00C	19
8	METHOD	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00L	298
7	METHOD	/IWCOR/CL_OD_HDLR_ROOT========CP	/IWCOR/CL_OD_HDLR_ROOT========CM00F	3
6	METHOD	/IWCOR/CL_REST_HTTP_HANDLER===CP	/IWCOR/CL_REST_HTTP_HANDLER===CM001	121
5	METHOD	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CP	/IWBEP/CL_OD_HTTP_REQ_HANDLER=CM007	70
4	METHOD	/IWBEP/CL_OD_ICF_HANDLER======CP	/IWBEP/CL_OD_ICF_HANDLER======CM001	39
3	METHOD	CL_HTTP_SERVER================CP	CL_HTTP_SERVER================CM00D	814
2	FUNCTION	SAPLHTTP_RUNTIME	LHTTP_RUNTIMEU02	1655
1	MODULE (PBO)	SAPMHTTP	SAPMHTTP	12



