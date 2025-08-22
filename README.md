<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" validated="{= ${viewModel>/step1_ok} ? true : false }">
        <HBox id="step1HBox" width="80%" justifyContent="Center">
          <Panel id="step1Panel" class="stepPanel" expandable="false"> <!-- headerText="1. Identification"> -->
            <content>
              <f:Form id="step1Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step1FormContainer">
                    <f:formElements>
                      <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                        <f:fields>
                          <Input id="ipContract" value="{Vbeln}" width="100%" maxLength="15"/>  
                          <!-- <macros:Field id="ipContract" contextPath="/ZI_WR_WEIGHINGSESSION" metaPath="Vbeln" readOnly="false"  /> -->
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
              <HBox id="step1Buttons" justifyContent="Start" width="80%" class="sapUiMediumMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press="onNextStep" visible="false"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type" validated="{= ${viewModel>/step2_ok} ? true : false }">
        <HBox id="step2HBox" width="80%" justifyContent="Center">
          <Panel id="step2Panel" class="stepPanel" expandable="false"> <!-- headerText="2. Choose Load Type"> -->
            <content>
              <VBox id="step2VBox">
                <!-- <Text id="step2Instruction" text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/> -->

                <!-- Big tappable buttons from VH entity -->
                <VBox id="ltContainer"
                      items="{
                        path: '/ZI_WR_LOADTYPEVH',
                        parameters: { $select: 'LoadType,LoadTypeText', $orderby: 'LoadType' }
                      }">
                  <Button
                    id="ltButton"
                    class="loadTypeBtn"
                    width="100%"
                    text="{= ${LoadType} + ' - ' + ${LoadTypeText} }"
                    press=".onChooseLoadType"/>
                </VBox>

                <!-- Show selection -->
                <Text id="step2SelectedText" class="sapUiSmallMarginTop"
                      text="{= ${binding>LoadType} ? 'Selected: ' + ${binding>LoadType} : ''}"/>

                <HBox id="step2Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                  <Button id="btnStep2Back" text="Back" press=".onBackToStep1"/>
                  <Button id="btnStep2Next" text="Next" type="Emphasized" press="onNextStep"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 3: Weighing -->
      <WizardStep id="step3" title="Weighing" validated="{= ${viewModel>/step3_ok} ? true : false }">
        <HBox id="step3HBox" width="80%" justifyContent="Center">
          <Panel id="step3Panel" class="stepPanel" expandable="false"> <!-- headerText="3. Weighing"> -->
            <content>
              <f:Form id="step3Form" editable="true">
                <f:layout><f:ResponsiveGridLayout id="tgrid1"/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step3FormContainer">
                    <f:formElements>
                      <f:FormElement id="step3FormElementGross" label="Gross Weight">
                        <f:fields>
                          <Input id="ipGrossWeight" value="{binding>GrossWeight}" width="100%" type="Number"/>
                        </f:fields>
                      </f:FormElement>
                      <f:FormElement id="step3FormElementTare" label="Tare Weight">
                        <f:fields>
                          <Input id="ipTareWeight" value="{binding>TareWeight}" width="100%" type="Number"/>
                        </f:fields>
                      </f:FormElement>
                      <f:FormElement id="step3FormElementNet" label="Net Weight">
                        <f:fields>
                          <Input id="ipNetWeight" value="{binding>NetWeight}" width="100%" type="Number" editable="false"/>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>

              <HBox id="step3Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                <Button id="btnStep3Back" text="Back" press=".onBackToStep2"/>
                <Button id="btnFinish" text="Finish" type="Accept" press="onNextStep"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>
 <!-- STEP 4: Complete and Print -->
      <WizardStep id="step4" title="Complete and Print" validated="false">
        <HBox id="step4HBox" width="80%" justifyContent="Center">
          <Panel id="step4Panel" class="stepPanel" expandable="false"> <!-- headerText="4. Complete and Print"> -->
            <content>
              <VBox id="step4VBox" class="sapUiSmallMargin">
                <!-- Session Summary -->
                <Text id="step4SummaryText"
                      text="Session Summary: {path: 'Sessionid', targetType: 'any'}"
                      class="sapUiSmallMarginBottom"/>

                <!-- Action buttons -->
                <HBox id="step4Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                  <Button id="btnStep4Print" text="Print Slip" type="Emphasized" press=".onPrintSlip"/>
                  <Button id="btnStep4Submit" text="Submit" type="Accept" press="onNextStep"/>
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
