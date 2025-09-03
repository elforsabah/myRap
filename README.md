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
@ObjectModel.resultSet.sizeCategory: #XS // Make it a Drop Down
define view entity ZI_WR_SALESITEM_CONTRACTVH
  with parameters
    P_SalesOrder : abap.char(10)
  as select distinct from I_SalesDocumentItem as salesitem
    inner join I_MaterialText as _text on salesitem.Material = _text.Material
                                       and _text.Language = $session.system_language
{
// @UI.hidden: true
    key salesitem.SalesDocument as SalesOrder,
// @UI.hidden: true
    key salesitem.SalesDocumentItem as SalesOrderitem,
    _text.Material as Material,
    _text.MaterialName as MaterialText,
// @UI.hidden: true
    _text.Language as Language
}
where salesitem.SalesDocument = $parameters.P_SalesOrder
