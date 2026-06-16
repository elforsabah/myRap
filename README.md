@AccessControl.authorizationCheck: #NOT_REQUIRED
@EndUserText.label: 'P&D Functional Location'
@Metadata.allowExtensions: true
@ObjectModel.usageType:{
  serviceQuality: #D,
  sizeCategory: #M,
  dataClass: #MIXED
}
define root view entity /PLCE/C_PDFunctionalLocation
  provider contract transactional_query
  as projection on /PLCE/R_PDFunctionalLocation
{
      @ObjectModel.text.element: [ 'FunctionalLocationDescription' ]
      @UI.textArrangement: #TEXT_SEPARATE
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLoc_VH', element: 'FunctionalLocation' },
                                           useForValidation: true }]
  key FunctionalLocation,
      @EndUserText.label: 'Functional Location Description'
      _FuctionalLocationText.FunctionalLocationDescription : localized,
      AddressId,
      @Consumption.filter.hidden: true
      Latitude,
      @Consumption.filter.hidden: true
      Longitude,
      Altitude,                                                                                
      @Consumption.filter: { multipleSelections: false, selectionType: #SINGLE, defaultValue: '', mandatory: false  }
      IsGeocoded,
      IsManualGeocode,
      @EndUserText.label: 'Geocoding Precision'
      @ObjectModel.text.element: ['GeocodingPrecisionText']
      GeocodePrecision,
      @UI.hidden: true
      _GeocodingPrecisionText.Description as GeocodingPrecisionText   : localized,
      AddressIsPersonAddress,
      AddresseeName1,
      AddresseeName2,
      StreetName,
      HouseNumber,
      HouseNumberSupplementText,
      CityName,
      PostalCode,
      Country,
      @Consumption.filter.hidden: true
      FullAddress,
      CreatedBy,
      CreatedAt,
      LastChangedBy,
      LastChangedAt,
      LocalLastChangedAt,
      @Consumption.filter: { multipleSelections: true, selectionType: #SINGLE } //defaultValue: '',
      @Consumption.valueHelpDefinition: [{ entity: { name: '/PLCE/C_PDFuncLocCategory_VH', element: 'FuncLocMDCategory'} }]
      @EndUserText.label: 'Functional Location Category'
      @ObjectModel.text.element: ['FuncLocMDCategoryName']
      FunctionalLocationMDCategory as FunctionalLocationMDCategory,
      //      _MDFunctionalLocation.FunctionalLocationMDCategory as FunctionalLocationMDCategory,
      _FuncLocMDCategoryText[1:Language = $session.system_language].FuncLocMDCategoryName,
      //      _MDFunctionalLocation._FuncLocMDCategoryText[1:Language = $session.system_language].FuncLocMDCategoryName
      /* Associations */
      _GeocodingPrecisionText
      //      _MDFunctionalLocation
}
