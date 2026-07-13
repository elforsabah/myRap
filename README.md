@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'Wertehilfe: Behältertyp'
@ObjectModel.resultSet.sizeCategory: #XS
@Search.searchable: true
define view entity ZI_ContainerTypeVH
  as select from etyp
{
      @Search.defaultSearchElement: true
      @Search.fuzzinessThreshold: 0.8
  key matnr   as ContainerType,
      bauform as BuildingForm,
      bautxt  as Description
}
where kombinat = 'B'
