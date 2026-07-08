static action CreateLanf parameter (
  ContractVbeln : vbeln_va;
  DeliveryDate : kbdat;
  CustomerRef : bstkd;
  Description : abap.char(225);
  Positions : array of {
    Matnr : matnr;
    Menge : dzmeng;
    Meins : meins;
  }
) result [1] (
  Vbeln : vbeln_va;
);

static action AttachDocument parameter (
  Vbeln : vbeln_va;
  Title : text100;
  Url : url;
) result [1] (
  Vbeln : vbeln_va;
);
