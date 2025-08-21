<!-- STEP 4: Complete and Print -->
<WizardStep id="step4" title="Complete and Print" validated="false">
  <HBox id="step4HBox" width="100%" justifyContent="Center">
    <Panel id="step4Panel" width="36rem" expandable="false" headerText="4. Complete and Print">
      <content>
        <VBox id="step4VBox" class="sapUiSmallMargin">
          <!-- Session Summary -->
          <Text id="step4SummaryText"
                text="Session Summary: {path: 'Sessionid', targetType: 'any'}"
                class="sapUiSmallMarginBottom"/>

          <!-- Action buttons -->
          <HBox id="step4Buttons" justifyContent="SpaceBetween" width="100%" class="sapUiSmallMarginTop">
            <Button id="btnStep4Print" text="Print Slip" press=".onPrintSlip"/>
            <Button id="btnStep4Submit" text="Submit" type="Accept" press=".onSubmit"/>
          </HBox>
        </VBox>
      </content>
    </Panel>
  </HBox>
</WizardStep>
