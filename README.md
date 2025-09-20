<!-- STEP 3: Weighing and the Weight -->
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
                  <!-- Row 1 -->
                  <f:FormElement id="step3FormElementInstruction" label="">
                    <f:fields>
                      <Text id="step3TextInstruction" text="Please make sure to place the vehicle correctly." class="sapUiSmallMarginTop"/>
                    </f:fields>
                  </f:FormElement>
                  <!-- Row 2 -->
                  <f:FormElement id="step3FormElementWeight" label="">
                  <f:fields>
                    <HBox id="step3HBoxWeight" width="100%" justifyContent="Start" class="bigWeightContainer">
                    <Text
                      id="step3TextWeight"
                      text="{local>/grossWeight}"
                      textAlign="Center"
                      wrapping="true"
                      class="bigWeightNumber"/>
                  </HBox>
                  <HBox id="step3HBoxWeight2" width="100%" justifyContent="Start" class="bigWeightContainer">
                    <Text
                      id="step3TextWeight2"
                      text="{local>/teraWeight}"
                      textAlign="Center"
                      wrapping="true"
                      class="bigWeightNumber"/>
                  </HBox>
                  </f:fields>
                </f:FormElement>
                  <!-- Row 3 -->
                  <f:FormElement id="step3FormElementConfirm" label="">
                    <f:fields>
                      <HBox id="step3HBoxConfirm" width="100%" justifyContent="Start" class="sapUiMediumMarginTop">
                        <Button id="step3BtnConfirm" width="8rem" text="CONFIRM"  press="onConfirmStep3" class="sapUiLargeText largeWeighButton" visible="false"/>
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
