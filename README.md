<f:Form id="step2Form" editable="true">
  <f:layout>
    <f:ResponsiveGridLayout
      labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
      adjustLabelSpan="false"/>
  </f:layout>
  <f:formContainers>
    <f:FormContainer>
      <f:formElements>
        <f:FormElement label="Load Type">
          <f:fields>
            <VBox id="ltContainer"
                  items="{
                    path: '/ZI_WR_SALESITEM_CONTRACTVH',
                    parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                  }">
              <Button id="ltButton"
                      class="loadTypeBtn"
                      width="100%"
                      text="{= ${Material} + ' - ' + ${MaterialText} }"
                      press=".onChooseLoadType"/>
            </VBox>
          </f:fields>
        </f:FormElement>

        <f:FormElement label="">
          <f:fields>
            <Text id="step2SelectedText"
                  text="{= ${binding>SalesOrder} ? 'Selected: ' + ${binding>SalesOrder} : ''}"/>
          </f:fields>
        </f:FormElement>
      </f:formElements>
    </f:FormContainer>
  </f:formContainers>
</f:Form>
