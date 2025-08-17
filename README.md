@EndUserText.label: 'VH: Status from domain ZDOM_STATUS'
@ObjectModel.dataCategory: #VALUE_HELP
define view entity ZC_StatusVH
  as select from I_DomainFixedValue
{
  key DomainValue  as Status,
      DomainText   as StatusText
}
where
  DomainName = 'ZDOM_STATUS'                // your domain name
  and Language = $session.system_language;
