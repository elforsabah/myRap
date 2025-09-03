@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Weighing Session'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHINGSESSION
  as select from zwr_weighsession
  association [0..*] to ZI_WR_SALESITEM_CONTRACTVH as _SalesItems
    on _SalesItems.SalesOrder = $projection.Vbeln
 
 
{
      key sessionid as Sessionid,
      vbeln as Vbeln,
      @UI.textArrangement: #TEXT_LAST
      loadtype as LoadType,
      @Semantics.quantity.unitOfMeasure: 'Grossweightunit'
      grossweight as Grossweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      grossweightunit as Grossweightunit,
      @Semantics.quantity.unitOfMeasure: 'Tareweightunit'
      tareweight as Tareweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      tareweightunit as Tareweightunit,
      @Semantics.quantity.unitOfMeasure: 'Netweightunit'
     
      netweight as Netweight,
      @Consumption.valueHelpDefinition: [{
      entity: { name: 'I_UnitOfMeasure', element: 'UnitOfMeasure' }
      }]
      netweightunit as Netweightunit,
      step as Step,
      step1ok as Step1Ok,
      step2ok as Step2Ok,
      issummited as IsSummited,
     
      _SalesItems
}
