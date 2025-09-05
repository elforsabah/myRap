@EndUserText.label: 'Print Slip'
define abstract entity ZAE_WR_WEIGHB_PRINT
  
{
     Vbeln : vbeln_va;
     Loadtype : matnr;
     @Semantics.quantity.unitOfMeasure: 'Grossweightunit'
     Weight : i; 
     weightunit : /WATP/DBCWEIGHTUNIT;
    
}
