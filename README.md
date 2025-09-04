<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" showNextButton="false">
 <!-- STEP 1: Identification -->
<WizardStep id="step1" title="Identification" validated="true" icon="sap-icon://business-card" >
  <HBox id="step1HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step1Rail" width="36rem">
      <Panel id="step1Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step1Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                id="step1FormLayout"
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer id="step1FormContainer">
                <f:formElements>

                  <!-- Row 1 -->
                  <f:FormElement id="step1FormElementInput" label="Please Enter your Contract ID">
                    <f:fields>
                      <Input id="step1InputContract"
                             value="{local>/contractId}"
                             width="100%"
                             maxLength="10"
                             required="true"
                             placeholder="Scan or enter Contract ID"
                             class="sapUiSizeCompact"
                             change=".onContractChange"/>
                    </f:fields>
                  </f:FormElement>

                  <!-- Row 2 -->
<!--                   <f:FormElement id="step1FormElementScan" label="">
                    <f:fields>
                      <HBox id="step1ScanRow" width="100%" justifyContent="Center" class="sapUiMediumMarginTop">
                        <Button id="step1BtnScanCard"
                                width="12rem"
                                type="Emphasized"
                                icon="sap-icon://business-card"
                                text="Scan Card"
                                press="onScanCard"/>
                      </HBox>
                    </f:fields>
                  </f:FormElement> -->

                  <!-- Custom Next Button -->
<!--                   <f:FormElement id="step1FormElementNext" label="">
                    <f:fields>
                      <HBox width="100%" justifyContent="End" class="sapUiMediumMarginTop">
                        <Button text="Next" type="Accept" press="onNextStep1" />
                      </HBox>
                    </f:fields>
                  </f:FormElement>
 -->
                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>

<!-- STEP 2: Choose Load Type -->
<WizardStep id="step2" title="Choose Load Type" validated="false" icon="sap-icon://sap-box">
  <HBox id="step2HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step2Rail" width="36rem">
      <Panel id="step2Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step2Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                id="step2FormLayout"
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer id="step2FormContainer">
                <f:formElements>

                  <!-- Row 1 -->
                  <f:FormElement id="step2FormElementLoadType" label="">
                    <f:fields>
                      <VBox id="step2LtContainer">
                      </VBox>
                    </f:fields>
                  </f:FormElement>

                  <!-- Row 2 -->
                  <f:FormElement id="step2FormElementSelection" label="">
                    <f:fields>
                      <Text id="step2SelectedText"
                            class="sapUiSmallMarginTop"
                            text="{= ${Vbeln} ? 'Selected Contract: ' + ${Vbeln} : ''}"/>
                    </f:fields>
                  </f:FormElement>

                  <!-- Custom Next Button -->
<!--                   <f:FormElement id="step2FormElementNext" label="">
                    <f:fields>
                      <HBox width="100%" justifyContent="End" class="sapUiMediumMarginTop">
                        <Button text="Next" type="Accept" press="onNextStep2" />
                      </HBox>
                    </f:fields>
                  </f:FormElement> -->

                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>
<WizardStep id="step3" title="Weighing" validated="false" icon="sap-icon://compare-2">
  <HBox id="step3HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step3Rail" width="36rem">
      <Panel id="step3Panel" class="stepPanel" expandable="false">
        <content>
          <f:Form id="step3Form" editable="true">
            <f:layout>
              <f:ResponsiveGridLayout
                id="step3FormLayout"
                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                adjustLabelSpan="false"/>
            </f:layout>
            <f:formContainers>
              <f:FormContainer id="step3FormContainer">
                <f:formElements>
                  <!-- Row 2 -->
                  <f:FormElement id="step3FormElementInstruction" label="">
                    <f:fields>
                      <Text id="step3TextInstruction" text="Please make sure to place the vehicle correctly." class="sapUiSmallMarginTop"/>
                      <HBox id="step3HBoxWeighButton" width="100%" justifyContent="Start" class="sapUiSmallMarginTop">
                        <Button id="step3BtnWeigh" width="100%" text="WEIGH" type="Emphasized" class="sapUiLargeText  largeWeighButton"/>
                      </HBox>
                    </f:fields>
                  </f:FormElement>
                  <!-- Row 3 -->
                  <f:FormElement id="step3FormElementWeight" label="">
                    <f:fields>
                      <HBox id="step3HBoxWeight" width="100%" justifyContent="Center">
                        <Text id="step3TextWeight" text="8.100 KG" class="sapUiLargeText"/>
                      </HBox>
                    </f:fields>
                  </f:FormElement>
                  <!-- Row 4 -->
                  <f:FormElement id="step3FormElementConfirm" label="">
                    <f:fields>
                      <HBox id="step3HBoxConfirm" width="100%" justifyContent="Start" class="sapUiMediumMarginTop">
                        <Button id="step3BtnConfirm" text="CONFIRM" type="Emphasized" press="onConfirmStep3"/>
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

<WizardStep id="step4" title="Printing" validated="false" icon="sap-icon://print">
  <HBox id="step4HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step4Rail" width="36rem">
      <!-- TODO: weighing content -->
      
      <!-- Custom Finish Button (add your content above this; adjust text/event as needed) -->
      <HBox id="asd" width="100%" justifyContent="End" class="sapUiMediumMarginTop">
        <Button id="asde" text="Finish" type="Accept" press="onFinishStep4" />
      </HBox>
    </VBox>
  </HBox>
</WizardStep>
</Wizard>
</content>
</Page>
</mvc:View>
