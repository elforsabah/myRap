<VBox id="step2LtContainer"
                            items="{
                              path: {= '/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder=%27' + ${Vbeln} + '%27)' },
                              parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                            }">
                        <Button id="step2LtButton"
                                class="loadTypeBtn"
                                width="100%"
                                text="{= ${Material} + ' - ' + ${MaterialText} }"
                                press=".onChooseLoadType"/>
                      </VBox>
