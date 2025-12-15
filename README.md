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
    P_SalesOrder : vbeln_va
  as select distinct from I_SalesDocumentItem as salesitem
    inner join            I_MaterialText      as _text on  salesitem.Material = _text.Material
                                                       and _text.Language     = $session.system_language
  association to /watp/tavvmara      as _tavvmara      on _text.Material = _tavvmara.matnr
  association to ZI_WR_R_WEIGHBRIDGE as WEIGHBRIDGEDOC on WEIGHBRIDGEDOC.SalesDocument = salesitem.SalesDocument
{
      //    @UI.hidden: true
  key salesitem.SalesDocument     as SalesOrder,
      //    @UI.hidden: true
  key salesitem.SalesDocumentItem as SalesOrderitem,
      _text.Material              as Material,
      _text.MaterialName          as MaterialText,
      _tavvmara.avvcode           as Avvcode,
      //    @UI.hidden: true
      _text.Language              as Language,
      WEIGHBRIDGEDOC.Korselsnr as Korselsnr,
      WEIGHBRIDGEDOC.Korselsnrindicator as Korselsnrindicator,
      _tavvmara
}
where
  salesitem.SalesDocument = $parameters.P_SalesOrder

