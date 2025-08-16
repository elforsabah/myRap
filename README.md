<Page title="Weighing Session Wizard" showNavButton="true">
    <content>
        <Wizard id="wizard" complete="onWizardComplete" stepActivate="onStepActivate">
            <steps>
                <WizardStep id="step1" title="Identification" validated="false">
                    <content>
                        <Panel headerText="Driver/Vehicle Identification">
                            <macros:Form id="identForm" metaPath="@com.sap.vocabularies.UI.v1.FieldGroup#Identification" />
                            <!-- Freestyle fallback if macros not sufficient: <Input value="{/vbeln}" placeholder="Enter Delivery Number" /> -->
                        </Panel>
                    </content>
                </WizardStep>
                <WizardStep id="step2" title="Load Type Selection" validated="false">
                    <content>
                        <Panel headerText="Select Load Type">
                            <Select selectedKey="{/load_type}" items="{path: '/LoadTypes'}" /> <!-- Assume a value help or static items -->
                            <!-- Use macros:Field for annotated selection fields -->
                        </Panel>
                    </content>
                </WizardStep>
                <WizardStep id="step3" title="Initial Weighing" validated="false">
                    <content>
                        <Panel headerText="Capture Initial Weights">
                            <macros:Form id="initialWeightsForm" metaPath="@com.sap.vocabularies.UI.v1.FieldGroup#Weights" />
                            <Button text="Initiate Weighing" press="onInitiateWeighing" /> <!-- Call to simulate/read weighbridge -->
                        </Panel>
                    </content>
                </WizardStep>
                <WizardStep id="step4" title="Unloading/Processing" validated="false">
                    <content>
                        <Panel headerText="Confirm Unloading">
                            <CheckBox text="Unloading Complete" selected="{/unloading}" />
                        </Panel>
                    </content>
                </WizardStep>
                <WizardStep id="step5" title="Final Weighing" validated="false">
                    <content>
                        <Panel headerText="Capture Final Weights">
                            <macros:Form id="finalWeightsForm" metaPath="@com.sap.vocabularies.UI.v1.FieldGroup#Weights" />
                            <Button text="Initiate Final Weighing" press="onFinalWeighing" />
                        </Panel>
                    </content>
                </WizardStep>
                <WizardStep id="step6" title="Summary & Submit" validated="false">
                    <content>
                        <Panel headerText="Review and Print">
                            <macros:Form id="summaryForm" metaPath="@com.sap.vocabularies.UI.v1.FieldGroup#Weights" editable="false" />
                            <Button text="Submit & Print Slip" press="onSubmit" />
                        </Panel>
                    </content>
                </WizardStep>
            </steps>
        </Wizard>
    </content>
    <footer>
        <Toolbar>
            <ToolbarSpacer />
            <Button text="Next" press="onNext" type="Emphasized" />
            <Button text="Previous" press="onPrevious" />
        </Toolbar>
    </footer>
</Page>
