@EndUserText.label: 'Input for LARF Creation'
define root abstract entity ZI_LARF_CREATE_INPUT {
  key DummyKey      : abap.char(1); // Required for root abstract entity
  ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  DeliveryDate      : kbdat;        // Delivery/Service date (FBUDA/KETDAT)
  CustomerRef       : bstkd;        // Customer reference (BSTKD)
  Description       : vbktext;      // Description (VBAK-TEXT)
  _Positions        : composition [0..*] of ZI_LARF_POSITION_INPUT;
}
