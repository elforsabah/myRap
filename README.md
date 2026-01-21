@EndUserText.label: 'Input for Document Attachment'
define root abstract entity ZI_ATTACH_INPUT

{
  key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // LANF number
  Title             : text100;      // Title/Description
  Url               : url;          // Document URL (for archive link)
    
}


@EndUserText.label: 'Input for LANF Creation'
define root abstract entity ZI_LANF_CREATE_INPUT
  
{
  key TechKey     : abap.char(1); // Required for root abstract entity
  ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  DeliveryDate      : kbdat;        // Delivery/Service date (FBUDA/KETDAT)
  CustomerRef       : bstkd;        // Customer reference (BSTKD)
  Description       : abap.char(225);      // Description (VBAK-TEXT)
  _Positions        : composition [0..*] of ZI_LANF_POSITION_INPUT;
    
}


@EndUserText.label: 'Position Input for LANF'
define abstract entity ZI_LANF_POSITION_INPUT
 
{
  key TechKey       : abap.char(1);
  Matnr             : matnr;        // Material number
@Semantics.quantity.unitOfMeasure: 'Meins'
Menge             : dzmeng;       // Quantity (ZMENG, assuming decimal quantity type)
@Semantics.unitOfMeasure: true
Meins             : meins;        // Unit of measure (ZIEME)

_Parent           : association to parent ZI_LANF_CREATE_INPUT;

}


@EndUserText.label: 'Response Structure'
define abstract entity ZI_LANF_RESPONSE
{
     key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // Created LANF number
  _Messages         : composition [0..*] of ZI_MESSAGE;
    
}

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
  
  _Parent           : association to parent zi_lanf_response;
}

abstract;
with hierarchy;

define behavior for ZI_ATTACH_INPUT //alias <alias_name>
{
}

abstract;
with hierarchy;

define behavior for ZI_LANF_CREATE_INPUT //alias <alias_name>
{


}


unmanaged implementation in class zbp_i_lanf_root unique;

define behavior for ZI_LANF_ROOT //alias <alias_name>
//persistent table ddddlsrc
lock master
authorization master ( instance )
//etag master <field_name>
{

 // First endpoint: Create LANF
  static function CreateLanf deep parameter ZI_LANF_CREATE_INPUT result [1] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
 static function AttachDocument deep parameter ZI_ATTACH_INPUT result [1] ZI_LANF_RESPONSE;


  create;
  update;
  delete;
}

CLASS lhc_zi_lanf_root DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.

    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR zi_lanf_root RESULT result.

    METHODS create FOR MODIFY
      IMPORTING entities FOR CREATE zi_lanf_root.

    METHODS update FOR MODIFY
      IMPORTING entities FOR UPDATE zi_lanf_root.

    METHODS delete FOR MODIFY
      IMPORTING keys FOR DELETE zi_lanf_root.

    METHODS read FOR READ
      IMPORTING keys FOR READ zi_lanf_root RESULT result.

    METHODS lock FOR LOCK
      IMPORTING keys FOR LOCK zi_lanf_root.

    METHODS attachdocument FOR READ
      IMPORTING keys FOR FUNCTION zi_lanf_root~attachdocument RESULT result.

    METHODS createlanf FOR READ
      IMPORTING keys FOR FUNCTION zi_lanf_root~createlanf RESULT result.

ENDCLASS.

CLASS lhc_zi_lanf_root IMPLEMENTATION.

  METHOD get_instance_authorizations.
  ENDMETHOD.

  METHOD create.
  ENDMETHOD.

  METHOD update.
  ENDMETHOD.

  METHOD delete.
  ENDMETHOD.

  METHOD read.
  ENDMETHOD.

  METHOD lock.
  ENDMETHOD.

  METHOD attachdocument.
  ENDMETHOD.

  METHOD createlanf.
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

