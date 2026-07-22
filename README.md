@EndUserText.label: 'Position Input for LANF'
define abstract entity ZI_LANF_POSITION_INPUT
{
  key ContractVbeln : vbeln_va;
  key Matnr         : matnr;
      @Semantics.quantity.unitOfMeasure: 'Meins'
      Menge         : dzmeng;
      @Semantics.unitOfMeasure: true
      Meins         : abap.char(3);      // was: meins -> conversion exit CUNIT

      _Parent : association to parent ZI_LANF_CREATE_INPUT
                  on $projection.ContractVbeln = _Parent.ContractVbeln;
}
