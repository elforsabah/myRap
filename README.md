@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Materials of Contract'
@ObjectModel.dataCategory: #VALUE_HELP
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZI_WR_SALESITEM_CONTRACTVH as select from I_SalesOrderItem as salesitem  
   inner join I_MaterialText as _text on salesitem.Material = _text.Material
                                      and _text.Language = $session.system_language
{
    key salesitem.SalesOrder as SalesOrder,
    key salesitem.SalesOrderItem as SalesOrderitem,
    _text.Material as Material,
    _text.MaterialName,
    _text.Language      
} 
