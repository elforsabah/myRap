@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.ignorePropagatedAnnotations: true
@EndUserText.label: 'VH: Load Type (logon language)'
@ObjectModel.dataCategory: #VALUE_HELP
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }
@ObjectModel.resultSet.sizeCategory: #XS  // Make it a Drop Down
define view entity ZI_WR_LOADTYPEVH 
   as select from I_DomainFixedValue as domainv
    // to-one association to the language-dependent text
    left outer join I_DomainFixedValueText as _Text
      on  _Text.SAPDataDictionaryDomain = domainv.SAPDataDictionaryDomain
      and _Text.DomainValue             = domainv.DomainValue
      and _Text.Language                = $session.system_language
{
  key domainv.DomainValue as LoadType,
      _Text.DomainText     as LoadTypeText
}
where domainv.SAPDataDictionaryDomain = 'LOAD_TYPE';



@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Weighing Session'
@Metadata.ignorePropagatedAnnotations: true
@Metadata.allowExtensions: true
define root view entity ZI_WR_WEIGHINGSESSION
  as select from zwr_weighsession
{
  key vbeln           as Vbeln,
  key sessionid       as Sessionid,
  
      @Consumption.valueHelpDefinition: [{ 
      entity: {
      name: 'ZI_WR_LOADTYPEVH',  
      element: 'LoadType'
      }}]
      
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
      issummited     as IsSummited
} 
