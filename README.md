@EndUserText.label: 'Input for LANF Creation'
define root abstract entity ZI_LANF_CREATE_INPUT {
  key TechKey       : abap.char(1); // Technical key for structure
  ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  DeliveryDate      : kbdat;        // Delivery/Service date (FBUDA/KETDAT)
  CustomerRef       : bstkd;        // Customer reference (BSTKD)
  Description       : vbktext;      // Description (VBAK-TEXT)
  _Positions        : composition [0..*] of ZI_LANF_POSITION_INPUT;
}


@EndUserText.label: 'Position Input for LANF'
define abstract entity ZI_LANF_POSITION_INPUT {
  key TechKey       : abap.char(1);
  Matnr             : matnr;        // Material number
  Menge             : dzmeng;       // Quantity (ZMENG, assuming decimal quantity type)
  Meins             : meins;        // Unit of measure (ZIEME)
}


@EndUserText.label: 'Response Structure'
define root abstract entity ZI_LANF_RESPONSE {
  key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // Created LANF number
  _Messages         : composition [0..*] of ZI_MESSAGE;
}

@EndUserText.label: 'Message Structure'
define abstract entity ZI_MESSAGE {
  key TechKey       : abap.char(1);
  Msgid             : msgid;
  Msgno             : msgnr;
  Msgty             : msgty;
  Msgv1             : symsgv;
  Msgv2             : symsgv;
  Msgv3             : symsgv;
  Msgv4             : symsgv;
}

@EndUserText.label: 'Input for Document Attachment'
define root abstract entity ZI_ATTACH_INPUT {
  key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // LANF number
  Title             : text100;      // Title/Description
  Url               : url;          // Document URL (for archive link)
}


unmanaged;

define behavior for ZI_LANF_ABSTRACT_ROOT // Dummy root abstract entity (define it as empty if needed)
{
  // First endpoint: Create LANF
  static function CreateLanf deep parameter ZI_LANF_CREATE_INPUT result [1] ZI_LANF_RESPONSE;

  // Second endpoint: Attach PDF/Document Link
  static function AttachDocument deep parameter ZI_ATTACH_INPUT result [1] ZI_LANF_RESPONSE;
}

@EndUserText.label: 'Dummy Root for LANF Behaviors'
define root abstract entity ZI_LANF_ABSTRACT_ROOT {
  key TechKey : abap.char(1);
}


ZI_LANF_POSITION_INPUT-MENGE Referenzinformationen fehlen oder falscher Datentyp, siehe Langtext
