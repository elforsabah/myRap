unmanaged implementation in class zbp_i_lanf_root unique;

define behavior for ZI_LANF_ROOT alias lanfroot
//persistent table ddddlsrc
lock master
authorization master ( instance )
//etag master <field_name>
{

 // First endpoint: Create LANF
  static action CreateLanf deep parameter ZI_LANF_CREATE_INPUT deep result [1] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
  static action AttachDocument  parameter ZI_ATTACH_INPUT deep result [1] ZI_LANF_RESPONSE;


//  create;
//  update;
//  delete;
}

abstract;
with hierarchy;

define behavior for ZI_LANF_RESPONSE alias response
{
 association _Messages;
 field( readonly ) TechKey;
}

define behavior for ZI_MESSAGE alias Msg
{
field( readonly ) TechKey;
association _Parent;
}

abstract;
//strict ( 2 );

with hierarchy;

define behavior for ZI_LANF_CREATE_INPUT alias createin
{
 association _Positions;
 field( readonly ) TechKey;
}

define behavior for ZI_LANF_POSITION_INPUT alias posIn
{

association _Parent;
field( readonly ) TechKey;
}

abstract;
strict ( 2 );

define behavior for ZI_ATTACH_INPUT //alias <alias_name>
{

}

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
  
  _Parent           : association to parent ZI_LANF_RESPONSE on $projection.TechKey = _Parent.TechKey;
}
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Root for LANF'
@Metadata.ignorePropagatedAnnotations: true
define root view entity ZI_LANF_ROOT as select from ddddlsrc
{
    
    key cast('' as abap.char(30)) as Techkey
} where 1 = 2 // Ensures no data is returned




@EndUserText.label: 'Response Structure'
define root abstract entity ZI_LANF_RESPONSE
{
     key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // Created LANF number
  _Messages         : composition [0..*] of ZI_MESSAGE ;
    
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

_Parent           : association to parent ZI_LANF_CREATE_INPUT  on $projection.TechKey = _Parent.TechKey;

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

@EndUserText.label: 'Input for Document Attachment'
define root abstract entity ZI_ATTACH_INPUT

{
  key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // LANF number
  Title             : text100;      // Title/Description
  Url               : url;          // Document URL (for archive link)
    
}



