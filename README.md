@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Materials of Contract'
@ObjectModel.dataCategory: #VALUE_HELP
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{ serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }
@ObjectModel.resultSet.sizeCategory: #XS
define view entity ZI_WR_SALESITEM_CONTRACTVH
  with parameters
    P_SalesOrder : vbeln_va
as select from I_SalesDocumentItem as salesitem
  inner join I_MaterialText      as _text
          on  salesitem.Material = _text.Material
          and _text.Language     = $session.system_language
  left outer join /watp/tavvmara as tav           // collapse AVV codes via aggregation
          on  tav.matnr           = _text.Material
  association to /watp/tavvmara      as _tavvmara
          on  _tavvmara.matnr      = _text.Material
  association to ZI_WR_R_WEIGHBRIDGE as WEIGHBRIDGEDOC
          on  WEIGHBRIDGEDOC.SalesDocument = salesitem.SalesDocument
{
  key salesitem.SalesDocument         as SalesOrder,
  key salesitem.SalesDocumentItem     as SalesOrderitem,
      _text.Material                  as Material,
      _text.MaterialName              as MaterialText,

      /* choose ONE AVV code; use MIN (or MAX) to collapse 1:n */
      min( tav.avvcode )              as Avvcode,

      _text.Language                  as Language,
      WEIGHBRIDGEDOC.Korselsnr        as Korselsnr,
      WEIGHBRIDGEDOC.Korselsnrindicator as Korselsnrindicator,

      /* keep the association; optionally filter it to the chosen Avvcode */
      _tavvmara[ avvcode = $projection.Avvcode ]  as _tavvmara
}
where salesitem.SalesDocument = $parameters.P_SalesOrder
group by
  salesitem.SalesDocument,
  salesitem.SalesDocumentItem,
  _text.Material,
  _text.MaterialName,
  _text.Language,
  WEIGHBRIDGEDOC.Korselsnr,
  WEIGHBRIDGEDOC.Korselsnrindicator
