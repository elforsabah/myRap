@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Materials of Contract'
@ObjectModel.dataCategory: #VALUE_HELP
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{ serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }
@ObjectModel.resultSet.sizeCategory: #XS
define view entity ZI_WR_SALESITEM_CONTRACTVH
  with parameters P_SalesOrder : vbeln_va
  as select from I_SalesDocumentItem as salesitem

    /* get material text; if this might be missing in some languages, switch to LEFT OUTER JOIN */
    inner join I_MaterialText as _text
      on  salesitem.Material = _text.Material
      and _text.Language     = $session.system_language

    /* aggregate AVV once per MATNR inside a derived table, then join it */
    left outer join (
      select from /watp/tavvmara as t
      {
        t.matnr                        as Material,
        max( t.avvcode )               as Avvcode   // or min(...) per your rule
      }
      group by t.matnr
    ) as v
      on v.Material = _text.Material

    /* keep many-side association only for drilldown */
    association to /watp/tavvmara      as _tavvmara
      on _tavvmara.matnr = _text.Material
    association to ZI_WR_R_WEIGHBRIDGE as WEIGHBRIDGEDOC
      on WEIGHBRIDGEDOC.SalesDocument = salesitem.SalesDocument

{
  key salesitem.SalesDocument        as SalesOrder,
  key salesitem.SalesDocumentItem    as SalesOrderitem,
      _text.Material                 as Material,
      _text.MaterialName             as MaterialText,
      v.Avvcode                      as Avvcode,          // single value per MATNR
      _text.Language                 as Language,
      WEIGHBRIDGEDOC.Korselsnr       as Korselsnr,
      WEIGHBRIDGEDOC.Korselsnrindicator as Korselsnrindicator,
      _tavvmara                                         // still navigable (many)
}
where salesitem.SalesDocument = $parameters.P_SalesOrder
