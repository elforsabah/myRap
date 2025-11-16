<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
    xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrigefinal.ext.main.Main" height="100%">
    <Page id="Main" class="myApp" >
      <customHeader>
    <Toolbar id="headerToolbar">
        <Image id="im1" src="/img/LogoMP.png" alt="Company Logo" width="100px" />
        <HBox id="headHBox1" width="100%" justifyContent="Center" class="bigWeightContainer">
        <Text id="txt1" text="Marius Pedersen" class="headText1" />
        </HBox>
        <ToolbarSpacer id="toole1"/>
          <Button id="btn15" icon="https://upload.wikimedia.org/wikipedia/en/thumb/a/ae/Flag_of_the_United_Kingdom.svg/1280px-Flag_of_the_United_Kingdom.svg.png"
                press=".onSetEnglish"
                type="Transparent"
                tooltip="English" /> <!-- Icon as flag; add text="EN" if desired -->
        <Button id="btn16" icon="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Flag_of_Denmark.svg/1280px-Flag_of_Denmark.svg.png"
                press=".onSetDanish"
                type="Transparent"
                tooltip="Danish" /> <!-- Icon as flag; add text="DK" if desired -->
    </Toolbar>
</customHeader>
<content>
  <Wizard id="weighingWizard" complete="onWizardComplete" showNextButton="false">
   <!-- STEP 1: Identification -->
<WizardStep id="step1" title="{@i18n>step1Title}" validated="true" icon="sap-icon://business-card" >
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
                  <f:FormElement id="step1FormElementInput" label="{@i18n>step1Label}">
                    <f:fields>
                      <Input id="step1InputContract"
                             value="{local>/contractId}"
                             width="100%"
                             maxLength="10"
                             required="true"
                             placeholder="{@i18n>step1Placeholder}"
                             class="sapUiSizeCompact"
                             change=".onContractChange"/>
                    </f:fields>
                  </f:FormElement>
                  <!-- Add Cancel Button -->
<!-- <f:FormElement id="step1FormElementCancel" label="">
                    <f:fields>
                      <HBox id="step1HBoxCancel" width="100%" justifyContent="Start" class="sapUiMediumMarginTop">
                        <Button id="step1BtnCancel" width="8rem" text="{@i18n>cancel}" press="onCancel" class="sapUiLargeText largeWeighButton"/>
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
<!-- STEP 2: Choose Load Type -->
<WizardStep id="step2" title="{@i18n>step2Title}" validated="false" icon="sap-icon://sap-box">
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
                            text="{= ${SalesDocument} ? ${@i18n>selectedContract} + ${SalesDocument} : ''}"/>
                    </f:fields>
                  </f:FormElement>
                  <!-- Add Cancel Button -->
                  <f:FormElement id="step2FormElementCancel" label="">
                    <f:fields>
                      <HBox id="step2HBoxCancel" width="100%" justifyContent="Start" class="sapUiMediumMarginTop">
                        <Button id="step2BtnCancel" width="8rem" text="{@i18n>cancel}" press="onCancel" class="sapUiLargeText largeWeighButton"/>
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
<!-- NEW OPTIONAL STEP: Korselsnr Identification (between step2 and step3) -->
<WizardStep id="stepK"
            title="{@i18n>stepKTitle}"
            validated="false"
            icon="sap-icon://business-card"
            visible="false">
    <HBox id="stepKHBoxOuter" width="100%" justifyContent="Center">
        <VBox id="stepKRail" width="36rem">
            <Panel id="stepKPanel" class="stepPanel" expandable="false">
                <content>
                    <f:Form id="stepKForm" editable="true">
                        <f:layout>
                            <f:ResponsiveGridLayout
                                id="stepKFormLayout"
                                labelSpanXL="3" labelSpanL="3" labelSpanM="3" labelSpanS="12"
                                adjustLabelSpan="false"/>
                        </f:layout>
                        <f:formContainers>
                            <f:FormContainer id="stepKFormContainer">
                                <f:formElements>
                                    <f:FormElement id="stepKFormElementInput" label="{@i18n>stepKLabel}">
                                        <f:fields>
                                            <Input id="stepKInputKorselsnr"
                                                   value="{local>/korselsnr}"
                                                   width="100%"
                                                   maxLength="10"
                                                   required="true"
                                                   placeholder="{@i18n>stepKPlaceholder}"
                                                   class="sapUiSizeCompact"
                                                   change=".onContractChange"/>
                                        </f:fields>
                                    </f:FormElement>
                                    <f:FormElement id="stepKFormElementCancel" label="">
                                        <f:fields>
                                            <HBox id="stepKHBoxCancel"
                                                  width="100%"
                                                  justifyContent="Start"
                                                  class="sapUiMediumMarginTop">
                                                <Button id="stepKBtnCancel"
                                                        width="8rem"
                                                        text="{@i18n>cancel}"
                                                        press="onCancel"
                                                        class="sapUiLargeText largeWeighButton"/>
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
<!-- STEP 3: Weighing and the Weight -->
<WizardStep id="step3" title="{@i18n>step3Title}" validated="false" icon="sap-icon://compare-2">
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
                  <!-- Row 1 -->
                  <f:FormElement id="step3FormElementInstruction" label="">
                    <f:fields>
                      <Text id="step3TextInstruction" text="{@i18n>step3Instruction}" class="sapUiSmallMarginTop"/>
                    </f:fields>
                  </f:FormElement>
                  <!-- Row 2 -->
                  <f:FormElement id="step3FormElementWeight" label="">
                    <f:fields>
                      <HBox id="step3HBoxWeight" width="100%" justifyContent="Center" class="bigWeightContainer">
                        <VBox id="step3VBoxWeights" class="tightWeights">
                          <Text
                            id="step3TextWeight"
                            text="{local>/grossWeight}"
                            textAlign="Center"
                            wrapping="true"
                            class="bigWeightNumber"/>
                          <Text
                            id="step3TextWeight2"
                            text="{local>/teraWeight}"
                            textAlign="Center"
                            wrapping="true"
                            class="bigWeightNumber"/>
                        </VBox>
                      </HBox>
                    </f:fields>
                  </f:FormElement>
                  <!-- Row 3 -->
                  <f:FormElement id="step3FormElementConfirm" label="">
                    <f:fields>
                      <HBox id="step3HBoxConfirm" width="100%" justifyContent="Start" class="sapUiMediumMarginTop">
                        <Button id="step3BtnConfirm" width="13rem" text="{@i18n>confirmAndPrint}" press="onConfirmStep3" class="sapUiLargeText largeWeighButton" visible="false"/>
                        <Button id="step3BtnConfirm2" width="8rem" text="{@i18n>confirm}" press="onConfirmStep3" class="sapUiLargeText largeWeighButton" visible="false"/>
                      </HBox>
                    </f:fields>
                  </f:FormElement>
                  <!-- Add Cancel Button -->
                  <f:FormElement id="step3FormElementCancel" label="">
                    <f:fields>
                      <HBox id="step3HBoxCancel" width="100%" justifyContent="Start" class="sapUiMediumMarginTop">
                        <Button id="step3BtnCancel" width="8rem" text="{@i18n>cancel}" press="onCancel" class="sapUiLargeText largeWeighButton"/>
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
</Wizard>
</content>
</Page>
</mvc:View>
