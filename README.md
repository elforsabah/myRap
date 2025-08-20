@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Materials of Contract'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZI_WR_SALESITEM_CONTRACT as select from I_SalesOrderItem as salesitem  
   inner join I_MaterialText as _text on salesitem.Material = _text.Material
                                      and _text.Language = $session.system_language
{
    key salesitem.SalesOrder as SalesOrder,
    key salesitem.SalesOrderItem as SalesOrderitem,
    salesitem._Material.Material as Material,
    salesitem._Material._Text.MaterialName,
    _text.Language      
} 
