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
                      <HBox id="step3HBoxWeighButton" width="100%" justifyContent="Center" class="sapUiSmallMarginTop">
                        <Button id="step3BtnWeigh" text="WEIGH" type="Emphasized" class="sapUiLargeText sapUiSizeCompact" style="width: 12rem; height: 4rem; font-size: 1.5rem;"/>
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
                      <HBox id="step3HBoxConfirm" width="100%" justifyContent="Center" class="sapUiMediumMarginTop">
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
      <!-- Custom Next Button -->
      <HBox id="ffg" width="100%" justifyContent="End" class="sapUiMediumMarginTop">
        <Button id="dgf" text="Next" type="Accept" press="onNextStep3" />
      </HBox>
    </VBox>
  </HBox>
</WizardStep>
