@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Weighing Session'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHINGSESSION
  as select from zwr_weighsession
  association [0..*] to ZI_WR_SALESITEM_CONTRACTVH as _SalesItems
    on _SalesItems.SalesOrder = $projection.Vbeln
  
  
{
@Consumption.valueHelpDefinition: [{
  entity: {
  name: 'ZI_WR_SALESITEM_CONTRACTVH',
  element: 'Material'
  },
  additionalBinding: [{
    localElement: 'Vbeln',
    element: 'SalesOrder',
    usage: #FILTER
}]
}]
  key vbeln           as Vbeln,
  key sessionid       as Sessionid,
      @UI.textArrangement: #TEXT_LAST
      loadtype       as LoadType,
      @Semantics.quantity.unitOfMeasure: 'Grossweightunit'
      grossweight     as Grossweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      grossweightunit as Grossweightunit,
      @Semantics.quantity.unitOfMeasure: 'Tareweightunit'
      tareweight      as Tareweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      tareweightunit  as Tareweightunit,
      @Semantics.quantity.unitOfMeasure: 'Netweightunit' 
      
      netweight       as Netweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      netweightunit   as Netweightunit,
      step            as Step,
      step1ok        as Step1Ok,
      step2ok        as Step2Ok,
      issummited     as IsSummited,
      
      _SalesItems
} 

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
@ObjectModel.resultSet.sizeCategory: #XS  // Make it a Drop Down
define view entity ZI_WR_SALESITEM_CONTRACTVH as select distinct from I_SalesDocumentItem as salesitem  
   inner join I_MaterialText as _text on salesitem.Material = _text.Material
                                      and _text.Language = $session.system_language
{
//    @UI.hidden: true
    key salesitem.SalesDocument as SalesOrder,
//    @UI.hidden: true
    key salesitem.SalesDocumentItem as SalesOrderitem,
    _text.Material as Material,
    _text.MaterialName as MaterialText,
//    @UI.hidden: true
    _text.Language as Language      
} 

