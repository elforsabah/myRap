@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Print Configuration'
@Metadata.allowExtensions: true
@Metadata.ignorePropagatedAnnotations: true
@Search.searchable: true
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define root view entity ZI_PRINTCONFIG
  provider contract transactional_query
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
      @UI.lineItem: [{ position: 50 }]
      filenameteemplate as Filenameteemplate
}
