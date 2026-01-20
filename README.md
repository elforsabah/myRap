@EndUserText.label: 'createAndProcess Payload - Item'
define abstract entity Z_A_SO_CREATE_REF_ITEM
{
  key ExternalRequestId       : abap.char(40);  "parent key included
  key SalesOrderItem          : abap.numc(6);

  ReferenceSDDocument         : vbeln_va;
  ReferenceSDDocumentItem     : posnr_va;

  RequestedQuantity           : kwmeng;
  RequestedQuantityUnit       : vrkme;
}
