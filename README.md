
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_SALESITEM_CONTRACTVH',
                        parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                      }">
                  <Button
