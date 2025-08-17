<VBox id="rootVBox" class="sapUiLargeMargin sapUiSmallContentPadding" width="100%">
  <!-- Create/Open draft session on load and bind context -->
  <macro:Form id="sessionHeader" metaPath="@com.sap.vocabularies.UI.v1.HeaderInfo" />

  <Wizard id="wiz" validateOnNext="true">
    <!-- Step 1: Identification -->
    <WizardStep id="step1" title="Identification" validated="{= ${binding>step1_ok} ? true : false }">
      <VBox id="vbStep1">
        <Label id="lblSalesDocument" text="Sales Document" required="true"/>
        <Input id="inpVbeln" value="{binding>vbeln}" width="20rem" />
        <HBox id="hbCard">
          <Input id="inpCardId" placeholder="Scan Card ID…" value="{view>/cardId}" width="20rem"/>
          <Button id="btnIdentify" text="Identify" press=".onIdentify"/>
        </HBox>
      </VBox>
    </WizardStep>

    <!-- Step 2: Load Type -->
    <WizardStep id="step2" title="Load type" validated="{= ${binding>step2_ok} ? true : false }">
      <VBox id="vbStep2">
        <Label id="lblLoadType" text="Load Type" required="true"/>
        <macro:Field id="fldLoadType" metaPath="load_type" /> <!-- uses CDS value help -->
        <Button id="btnConfirmLoadType" text="Confirm Load Type" press=".onSetLoadType"/>
      </VBox>
    </WizardStep>

    <!-- Step 3: Weighing -->
    <WizardStep id="step3" title="Weighing">
      <VBox id="vbStep3">
        <HBox id="hbGross">
          <Button id="btnGetGross" text="Get Gross" press=".onGetGross"/>
          <Text id="txtGross" text="{= ${binding>grossweight} ? ${binding>grossweight} + ' ' + ${binding>grossweightunit} : '' }"/>
        </HBox>
        <HBox id="hbTare">
          <Button id="btnGetTare" text="Get Tare" press=".onGetTare"/>
          <Text id="txtTare" text="{= ${binding>tareweight} ? ${binding>tareweight} + ' ' + ${binding>tareweightunit} : '' }"/>
        </HBox>
        <HBox id="hbFinalize">
          <Button id="btnFinalizePrint" text="Finalize & Print Slip" type="Emphasized" press=".onFinalizeAndPrint"/>
          <Text id="txtNet" text="{= ${binding>netweight} ? 'Net: ' + ${binding>netweight} + ' ' + ${binding>netweightunit} : '' }"/>
        </HBox>
      </VBox>
    </WizardStep>
  </Wizard>

  <Toolbar id="tbFooter">
    <ToolbarSpacer id="tbFooterSpacer"/>
    <Button id="btnOpenOP" text="Open Object Page" press=".onOpenOP"/>
  </Toolbar>
</VBox>
