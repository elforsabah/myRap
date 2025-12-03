@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Interface View for ZWR_CWAPRTCONF'
@Metadata.ignorePropagatedAnnotations: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZI_PRINTCONFIG
  as select from zwr_cwaprtconf
{

  @UI.selectionField: [{ position: 10 }]
  @UI.lineItem: [{ position: 10 }]
  key field             as Field,
  @UI.lineItem: [{ position: 20 }]
  key printform         as Printform,
  @UI.lineItem: [{ position: 30 }]
  key formtype          as Formtype,
  @UI.lineItem: [{ position: 40 }]
      parameter_in      as ParameterIn,
      @UI.lineItem: [{ position: 500 }]
      filenameteemplate as Filenameteemplate
}
