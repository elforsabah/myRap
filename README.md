@EndUserText.label: 'createAndProcess Payload - Header'
define root abstract entity Z_A_SO_CREATE_REF
{
  key ExternalRequestId     : abap.char(40);   // idempotency key from BMS (your choice)

  SalesOrderType            : auart;           // like PDF: SalesOrderType :contentReference[oaicite:4]{index=4}
  ReferenceSDDocument       : vbeln_va;        // like PDF: ReferenceSDDocument :contentReference[oaicite:5]{index=5}

  PurchaseOrderByCustomer   : bstkd;           // optional (PDF uses it in examples) :contentReference[oaicite:6]{index=6}
  ServiceDate               : dats;            // your service date

  PdfTitle                  : abap.char(80);   // optional
  PdfUrl                    : abap.char(255);  // optional

  composition [0..*] of Z_A_SO_CREATE_REF_ITEM as to_Item;
}
