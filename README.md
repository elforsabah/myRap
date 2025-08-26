
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_SALESITEM_CONTRACTVH',
                        parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                      }">
                  <Button//          @Consumption.valueHelpDefinition: [{
//          entity: {
//          name: 'ZI_WR_LOADTYPEVH',
//          element: 'LoadType'
//          }
//
//          }]
      
          @Consumption.valueHelpDefinition: [{
          entity: {
          name: 'ZI_WR_SALESITEM_CONTRACTVH',
          element: 'LoadType'
          }

          }]
