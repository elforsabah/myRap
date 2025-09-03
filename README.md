<WizardStep id="step1" title="Identification">
  <HBox width="100%" justifyContent="Center">
    <VBox width="36rem"> <!-- Centered rail -->
      <Panel id="step1Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step1Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer>
                <f:formElements>
                  
                  <!-- Row 1: Label + input -->
                  <f:FormElement label="Please Enter your Contract ID">
                    <f:fields>
                      <Input id="ipContract"
                             value="{Vbeln}"
                             width="100%"
                             maxLength="10"
                             required="true"
                             placeholder="Scan or enter Contract ID"
                             class="sapUiSizeCompact"
                             change=".onContractChange"/>
                    </f:fields>
                  </f:FormElement>
                  
                  <!-- Row 2: (no label) Scan button aligned to the same field column -->
                  <f:FormElement label="">
                    <f:fields>
                      <HBox width="100%" justifyContent="Center" class="sapUiMediumMarginTop">
                        <Button id="btnScanCard"
                                width="12rem"
                                type="Emphasized"
                                icon="sap-icon://business-card"
                                text="Scan Card"
                                press="onScanCard"/>
                      </HBox>
                    </f:fields>
                  </f:FormElement>

                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>


<WizardStep id="step2" title="Choose Load Type">
  <HBox width="100%" justifyContent="Center">
    <VBox width="36rem"> <!-- Same centered rail -->
      <Panel id="step2Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step2Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer>
                <f:formElements>

                  <!-- Row 1: Label + list of load-type buttons inside field column -->
                  <f:FormElement label="Load Type">
                    <f:fields>
                      <VBox id="ltContainer"
                            items="{
                              path: '/ZI_WR_SALESITEM_CONTRACTVH',
                              parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                            }">
                        <Button id="ltButton"
                                class="loadTypeBtn"
                                width="100%"  <!-- or 20rem if you prefer fixed button width -->
                                text="{= ${Material} + ' - ' + ${MaterialText} }"
                                press=".onChooseLoadType"/>
                      </VBox>
                    </f:fields>
                  </f:FormElement>

                  <!-- Row 2: selection info (no label) aligned with field column -->
                  <f:FormElement label="">
                    <f:fields>
                      <Text id="step2SelectedText" class="sapUiSmallMarginTop"
                            text="{= ${binding>SalesOrder} ? 'Selected: ' + ${binding>SalesOrder} : ''}"/>
                    </f:fields>
                  </f:FormElement>

                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>
