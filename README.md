@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.ignorePropagatedAnnotations: true
@EndUserText.label: 'VH: Status from domain Load_type'
@ObjectModel.dataCategory: #VALUE_HELP
@ObjectModel.usageType:{
    serviceQuality: #X,
    sizeCategory: #S,
    dataClass: #MIXED
}
define view entity ZI_WR_LOADTYPE 
  as select from I_DomainFixedValue as domainv
  
  association [0..*] to I_DomainFixedValueText as _DomainFixedValueText on  domainv.SAPDataDictionaryDomain = _DomainFixedValueText.SAPDataDictionaryDomain                                                                       
                                                                         and _DomainFixedValueText.Language  = $session.system_language
{
  key domainv.DomainValue  as Status,
      domainv._DomainFixedValueText.DomainText   as StatusText
}
where
  domainv.SAPDataDictionaryDomain = 'LOAD_TYPE' 
  

  
  
