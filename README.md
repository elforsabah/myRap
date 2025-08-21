<mvc:View
  controllerName="your.namespace.controller.Wizard"
  xmlns:mvc="sap.ui.core.mvc"
  xmlns="sap.m"
  xmlns:f="sap.ui.layout.form"
  xmlns:core="sap.ui.core"
  xmlns:macros="sap.fe.macros">

  <VBox id="pageRoot" width="100%">
    <!-- Optional app header -->
    <Toolbar id="appHeader">
      <Title id="appTitle" text="Weighbridge Integration" level="H4"/>
      <ToolbarSpacer/>
    </Toolbar>

    <!-- Wizard -->
    <Wizard id="wiz" validateOnNext="true">

      <!-- STEP 1: Identification -->
      <WizardStep id="step1" title="Identification" validated="{= ${viewModel>/step1_ok} ? true : false }">
        <HBox id="step1HBox" width="100%" justifyContent="Center">
          <Panel id="step1Panel" class="stepPanel" expandable="false" headerText="1. Identification">
            <content>
              <f:Form id="step1Form" editable="true">
                <f:layout><f:ResponsiveGridLayout/></f:layout>
                <f:formContainers>
                  <f:FormContainer id="step1FormContainer">
                    <f:formElements>
                      <f:FormElement id="step1FormElementContract" label="Please Enter your Contract ID">
                        <f:fields>
                          <Input id="ipContract" value="{binding>vbeln}" width="100%" maxLength="15"/>
                        </f:fields>
                      </f:FormElement>
                    </f:formElements>
                  </f:FormContainer>
                </f:formContainers>
              </f:Form>

              <HBox id="step1Buttons" justifyContent="End" width="100%" class="sapUiMediumMarginTop">
                <Button id="btnStep1Next" text="Next" type="Emphasized" press=".onStep1Next"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 2: Choose Load Type -->
      <WizardStep id="step2" title="Choose Load Type" validated="{= ${viewModel>/step2_ok} ? true : false }">
        <HBox id="step2HBox" width="100%" justifyContent="Center">
          <Panel id="step2Panel" class="stepPanel" expandable="false" headerText="2. Choose Load Type">
            <content>
              <VBox id="step2VBox">
                <Text id="step2Instruction" text="Position vehicle on weighbridge" class="sapUiSmallMarginBottom"/>

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
                  <Button id="btnStep2Next" text="Next" type="Emphasized" press=".onStep2Next"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 3: Weighing -->
      <WizardStep id="step3" title="Weighing" validated="{= ${viewModel>/step3_ok} ? true : false }">
        <HBox id="step3HBox" width="100%" justifyContent="Center">
          <Panel id="step3Panel" class="stepPanel" expandable="false" headerText="3. Weighing">
            <content>
              <f:Form id="step3Form" editable="true">
                <f:layout><f:ResponsiveGridLayout/></f:layout>
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
                <Button id="btnFinish" text="Finish" type="Accept" press=".onFinish"/>
              </HBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

      <!-- STEP 4: Complete and Print -->
      <WizardStep id="step4" title="Complete and Print" validated="false">
        <HBox id="step4HBox" width="100%" justifyContent="Center">
          <Panel id="step4Panel" class="stepPanel" expandable="false" headerText="4. Complete and Print">
            <content>
              <VBox id="step4VBox" class="sapUiSmallMargin">
                <!-- Session Summary -->
                <Text id="step4SummaryText"
                      text="Session Summary: {path: 'Sessionid', targetType: 'any'}"
                      class="sapUiSmallMarginBottom"/>

                <!-- Action buttons -->
                <HBox id="step4Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiMediumMarginTop">
                  <Button id="btnStep4Print" text="Print Slip" type="Emphasized" press=".onPrintSlip"/>
                  <Button id="btnStep4Submit" text="Submit" type="Accept" press=".onSubmit"/>
                </HBox>
              </VBox>
            </content>
          </Panel>
        </HBox>
      </WizardStep>

    </Wizard>
  </VBox>
</mvc:View>
