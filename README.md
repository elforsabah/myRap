<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >
 <!-- STEP 1: Identification -->
<WizardStep id="step1" title="Identification">
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
                  <f:FormElement id="step1FormElementScan" label="">
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

<!-- STEP 2: Choose Load Type -->
<WizardStep id="step2" title="Choose Load Type">
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

                </f:formElements>
              </f:FormContainer>
            </f:formContainers>
          </f:Form>
        </content>
      </Panel>
    </VBox>
  </HBox>
</WizardStep>
<WizardStep id="step3" title="Weighing">
  <HBox id="step3HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step3Rail" width="36rem">
      <!-- TODO: weighing content -->
    </VBox>
  </HBox>
</WizardStep>

<WizardStep id="step4" title="Printing">
  <HBox id="step4HBoxOuter" width="100%" justifyContent="Center">
    <VBox id="step4Rail" width="36rem">
      <!-- TODO: weighing content -->
    </VBox>
  </HBox>
</WizardStep>
</Wizard>
</content>
</Page>
</mvc:View>
