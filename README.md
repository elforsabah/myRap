@EndUserText.label: 'Position Input for LANF'
define abstract entity ZI_LANF_POSITION_INPUT
 
{
  key ContractVbeln     : vbeln_va;     // Contract number (VBELN)
  key Matnr             : matnr;        // Material number
@Semantics.quantity.unitOfMeasure: 'Meins'
Menge             : dzmeng;       // Quantity (ZMENG, assuming decimal quantity type)
@Semantics.unitOfMeasure: true
Meins             : abap.unit(3);         //meins; Unit of measure (ZIEME)

_Parent           : association to parent ZI_LANF_CREATE_INPUT  on $projection.ContractVbeln = _Parent.ContractVbeln;

}


