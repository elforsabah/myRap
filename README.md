<mvc:View
  xmlns:core="sap.ui.core"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns="sap.m"
  xmlns:f="sap.ui.layout.form"
  xmlns:html="http://www.w3.org/1999/xhtml"
  controllerName="com.prologa.zwrweighbrigefinal.ext.main.Main"
  height="100%">

  <Page id="Main" class="myApp">
    <content>
      <Wizard id="weighingWizard" complete="onWizardComplete">

        <!-- STEP 1: Identification -->
        <WizardStep id="step1" title="Identification">
          <HBox width="100%" justifyContent="Center">
            <Panel class="stepPanel" expandable="false">
              <content>
                <f:Form id="step1Form" editable="true">
                  <f:layout><f:ResponsiveGridLayout/></f:layout>
                  <f:formContainers>
                    <f:FormContainer>
                      <f:formElements>
                        <f:FormElement label="Please Enter your Contract ID">
                          <f:fields>

                            <!-- bind to local JSON model, not to computed SalesDocument -->
                            <Input id="ipContract"
                                   value="{view>/contractId}"
                                   width="80%"
                                   maxLength="10"
                                   required="true"
                                   placeholder="Scan or enter Contract ID"
                                   class="sapUiSizeCompact"
                                   change=".onContractChange"/>

                            <HBox width="100%" justifyContent="SpaceBetween" class="sapUiMediumMarginTop">
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

                <HBox justifyContent="Start" width="100%" class="sapUiMediumMarginTop">
                  <Button id="btnStep1Next" text="Next" type="Emphasized" press="onNextStep" visible="false"/>
                </HBox>
              </content>
            </Panel>
          </HBox>
        </WizardStep>

        <!-- STEP 2: Choose Load Type -->
        <WizardStep id="step2" title="Choose Load Type">
          <HBox width="100%" justifyContent="Center">
            <Panel class="stepPanel" expandable="false">
              <content>
                <VBox>

                  <!-- ValueHelp entity read-only list -->
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

                  <!-- (remove undefined model alias 'binding>' to avoid resolution errors) -->
                  <Text id="step2SelectedText" class="sapUiSmallMarginTop" text=""/>

                  <HBox id="step2Buttons" justifyContent="SpaceBetween" width="80%" class="sapUiMediumMarginTop">
                    <Button id="btnStep2Back" text="Back" press=".onBackToStep1"/>
                    <Button id="btnStep2Next" text="Next" type="Emphasized" press="onNextStep"/>
                  </HBox>

                </VBox>
              </content>
            </Panel>
          </HBox>
        </WizardStep>

      </Wizard>
    </content>
  </Page>
</mvc:View>
