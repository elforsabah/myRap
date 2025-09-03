<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" >
        <HBox id="step1HBox" width="100%" justifyContent="Center">
          <Panel id="step1Panel" class="stepPanel" expandable="false"> <!-- headerText="1. Identification"> -->
            <content>
              <f:Form id="step1Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step1FormContainer">
                    <f:formElements>
                      <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                        <f:fields>
                         <Input id="ipContract" value="{Vbeln}" width="80%" maxLength="10" required="true" placeholder="Scan or enter Contract ID" class="sapUiSizeCompact" change=".onContractChange"/>   
                          <HBox id="step1ScanRow" width="100%" justifyContent="SpaceBetween" class="sapUiMediumMarginTop">
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
              <HBox id="step1Buttons" justifyContent="Start" width="100%" class="sapUiMediumMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press="onNextStep" visible="false"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type"> <!--  validated="{= ${viewModel>/step2_ok} ? true : false }" -->
        <HBox id="step2HBox" width="100%" justifyContent="Center">
          <Panel id="step2Panel" class="stepPanel" expandable="false"> <!-- headerText="2. Choose Load Type"> -->
            <content>
              <VBox id="step2VBox">
                <!-- <Text id="step2Instruction" text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/> -->
                <!-- Big tappable buttons from VH entity -->
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_SALESITEM_CONTRACTVH',
                        parameters: { $select: 'SalesOrder,SalesOrderitem,Material,MaterialText,Language', $orderby: 'SalesOrder,SalesOrderitem' }
                      }">
                  <Button
                    id="ltButton"
                    class="loadTypeBtn"
                    width="100%"
                    text="{= ${Material} + ' - ' + ${MaterialText} }"
                    press=".onChooseLoadType"/>
                </VBox>

                <!-- Show selection -->
                <Text id="step2SelectedText" class="sapUiSmallMarginTop"
                      text="{= ${binding>SalesOrder} ? 'Selected: ' + ${binding>SalesOrder} : ''}"/>

                <HBox id="step2Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
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
