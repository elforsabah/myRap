<!-- NEW OPTIONAL STEP: Korselsnr Identification (between step2 and step3) -->
<WizardStep id="stepK"
            title="{@i18n>stepKTitle}"
            validated="false"
            icon="sap-icon://business-card"
            visible="{local>/showStepK}">
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
