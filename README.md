@ObjectModel.dataCategory: #VALUE_HELP
define view entity ZI_WR_SALESITEM_CONTRACTVH
  with parameters
    P_SalesOrder : vbeln_va
as select from I_SalesDocumentItem as salesitem
  inner join I_MaterialText as _text
    on  salesitem.Material = _text.Material
    and _text.Language     = $session.system_language

  inner join /watp/tavvmara as tavv
    on tavv.matnr = salesitem.Material
{
  key salesitem.SalesDocument      as SalesOrder,
  key salesitem.SalesDocumentItem  as SalesOrderItem,
  key salesitem.Material           as Material,
  key tavv.avvcode                 as Avvcode,

      _text.MaterialName           as MaterialText,
      _text.Language              as Language

}
where salesitem.SalesDocument = $parameters.P_SalesOrder;
