@Consumption.valueHelpDefinition: [{
    valueList:     '/SIEC/SD_I_CustomerReferenceVH',   // your VH entity
    valueListType: #FIXED_VALUES,                      // <- forces drop-down
    labelElement:  'CustomerReferenceText',            // text property in VH
    additionalBinding: [
      { localElement: 'CustomerReference', valueListProperty: 'CustomerReference' }
    ]
}]
