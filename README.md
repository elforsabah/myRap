<VBox class="sapUiLargeMargin sapUiSmallContentPadding" width="100%">
    <!-- Create/Open draft session on load and bind context -->
    <macro:Form id="sessionHeader" metaPath="@com.sap.vocabularies.UI.v1.HeaderInfo" />

    <Wizard id="wiz" validateOnNext="true">
      <!-- Step 1: Identification -->
      <WizardStep id="step1" title="Identification" validated="{= ${binding>step1_ok} ? true : false }">
        <VBox>
          <Label text="Sales Document" required="true"/>
          <Input value="{binding>vbeln}" width="20rem" />
          <HBox>
            <Input placeholder="Scan Card ID…" value="{view>/cardId}" width="20rem"/>
            <Button text="Identify" press=".onIdentify"/>
          </HBox>
        </VBox>
      </WizardStep>

      <!-- Step 2: Load Type -->
      <WizardStep id="step2" title="Load type" validated="{= ${binding>step2_ok} ? true : false }">
        <VBox>
          <Label text="Load Type" required="true"/>
          <macro:Field id="loadType" metaPath="load_type" /> <!-- uses CDS value help -->
          <Button text="Confirm Load Type" press=".onSetLoadType"/>
        </VBox>
      </WizardStep>

      <!-- Step 3: Weighing -->
      <WizardStep id="step3" title="Weighing">
        <VBox>
          <HBox>
            <Button text="Get Gross" press=".onGetGross"/>
            <Text text="{= ${binding>grossweight} ? ${binding>grossweight} + ' ' + ${binding>grossweightunit} : '' }"/>
          </HBox>
          <HBox>
            <Button text="Get Tare" press=".onGetTare"/>
            <Text text="{= ${binding>tareweight} ? ${binding>tareweight} + ' ' + ${binding>tareweightunit} : '' }"/>
          </HBox>
          <HBox>
            <Button text="Finalize & Print Slip" type="Emphasized" press=".onFinalizeAndPrint"/>
            <Text text="{= ${binding>netweight} ? 'Net: ' + ${binding>netweight} + ' ' + ${binding>netweightunit} : '' }"/>
          </HBox>
        </VBox>
      </WizardStep>
    </Wizard>

    <Toolbar>
      <ToolbarSpacer/>
      <Button text="Open Object Page" press=".onOpenOP"/>
    </Toolbar>
  </VBox>
