@AbapCatalog.viewEnhancementCategory: [#NONE]
@AccessControl.authorizationCheck: #NOT_REQUIRED
@Metadata.ignorePropagatedAnnotations: true
@EndUserText.label: 'VH: Load Type (logon language)'
@ObjectModel.dataCategory: #VALUE_HELP
@ObjectModel.usageType: { serviceQuality: #X, sizeCategory: #S, dataClass: #MIXED }
define view entity ZI_WR_LOADTYPE
  as select from I_DomainFixedValue as domainv
    // to-one association to the language-dependent text
    left outer to one association [0..1] to I_DomainFixedValueText as _Text
      on  _Text.SAPDataDictionaryDomain = domainv.SAPDataDictionaryDomain
      and _Text.DomainValue             = domainv.DomainValue
      and _Text.Language                = $session.system_language
{
  key domainv.DomainValue as Status,
      _Text.DomainText     as StatusText
}
where domainv.SAPDataDictionaryDomain = 'LOAD_TYPE';

  
