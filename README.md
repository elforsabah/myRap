Updated CDS Entities
Input for Create LANF (Debit Memo Request) - No Change Needed Here
abap@EndUserText.label: 'Input for LANF Creation'
define root abstract entity ZI_LANF_CREATE_INPUT {
  key TechKey       : abap.char(1); // Technical key for structure
  ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  DeliveryDate      : kbdat;        // Delivery/Service date (FBUDA/KETDAT)
  CustomerRef       : bstkd;        // Customer reference (BSTKD)
  Description       : vbktext;      // Description (VBAK-TEXT)
  _Positions        : composition [0..*] of ZI_LANF_POSITION_INPUT;
}
Position Input for LANF - Added To-Parent Association
abap@EndUserText.label: 'Position Input for LANF'
define abstract entity ZI_LANF_POSITION_INPUT {
  key TechKey       : abap.char(1);
  Matnr             : matnr;        // Material number
  
  @Semantics.quantity.unitOfMeasure: 'Meins'
  Menge             : dzmeng;       // Quantity (ZMENG, assuming decimal quantity type)
  
  @Semantics.unitOfMeasure: true
  Meins             : meins;        // Unit of measure (ZIEME)
  
  _Parent           : association to parent ZI_LANF_CREATE_INPUT;
}
Output for Responses (VBELN + Messages) - No Change Needed Here
abap@EndUserText.label: 'Response Structure'
define root abstract entity ZI_LANF_RESPONSE {
  key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // Created LANF number
  _Messages         : composition [0..*] of ZI_MESSAGE;
}
Message Structure - Added To-Parent Association
abap@EndUserText.label: 'Message Structure'
define abstract entity ZI_MESSAGE {
  key TechKey       : abap.char(1);
  Msgid             : msgid;
  Msgno             : msgnr;
  Msgty             : msgty;
  Msgv1             : symsgv;
  Msgv2             : symsgv;
  Msgv3             : symsgv;
  Msgv4             : symsgv;
  
  _Parent           : association to parent ZI_LANF_RESPONSE;
}
Input for Attach Document (Second Endpoint) - No Change Needed (No Composition)
abap@EndUserText.label: 'Input for Document Attachment'
define root abstract entity ZI_ATTACH_INPUT {
  key TechKey       : abap.char(1);
  Vbeln             : vbeln_va;     // LANF number
  Title             : text100;      // Title/Description
  Url               : url;          // Document URL (for archive link)
}
