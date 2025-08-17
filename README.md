<mvc:View xmlns="sap.m" xmlns:mvc="sap.ui.core.mvc" xmlns:f="sap.ui.layout.form" xmlns:core="sap.ui.core" controllerName="com.example.weighingsessionwizard.controller.CustomPage">
    <Page title="Weighing Session Wizard">
        <content>
            <Wizard id="weighingWizard" complete="onWizardComplete" enableNextButton="false">
                <!-- Step 1: Identification (as in screenshot 3) -->
                <WizardStep id="step1" title="Identification" validated="false" activate="onStepActivate">
                    <f:Form>
                        <f:layout>
                            <f:ResponsiveGridLayout />
                        </f:layout>
                        <f:formContainers>
                            <f:FormContainer title="Contract Details">
                                <f:formElements>
                                    <f:FormElement label="Sales Document">
                                        <f:fields>
                                            <Input value="{Vbeln}" />  <!-- Bound to CDS view field -->
                                        </f:fields>
                                    </f:FormElement>
                                    <f:FormElement label="Item">
                                        <f:fields>
                                            <Input value="{Item}" />
                                        </f:fields>
                                    </f:FormElement>
                                    <!-- Add ID Card scan or other identification fields -->
                                </f:formElements>
                            </f:FormContainer>
                        </f:formContainers>
                    </f:Form>
                    <Button text="Next" press="onNextStep" />
                </WizardStep>
                
                <!-- Step 2: Position Vehicle and Select Load Type -->
                <WizardStep id="step2" title="Load Selection" validated="false">
                    <VBox>
                        <Label text="Position vehicle on weighbridge" />
                        <Select id="loadTypeSelect" selectedKey="{LoadType}">
                            <items>
                                <core:Item key="Inbound" text="Inbound" />
                                <core:Item key="Outbound" text="Outbound" />
                            </items>
                        </Select>
                        <!-- Bind to CDS fields like LoadType -->
                    </VBox>
                    <Button text="Next" press="onNextStep" />
                </WizardStep>
                
                <!-- Step 3: Initiate Weighing (Gross Weight) -->
                <WizardStep id="step3" title="Initial Weighing" validated="false">
                    <Input value="{GrossWeight}" description="{GrossWeightUnit}" />
                    <!-- Use Fiori Elements Building Block for quantity if needed: <macros:Field definition="{path: 'GrossWeight'}" /> -->
                    <Button text="Capture Weight" press="onCaptureWeight" />
                </WizardStep>
                
                <!-- Step 4: Unloading/Select Load (if applicable) -->
                <WizardStep id="step4" title="Unloading" validated="false">
                    <!-- Add unloading details, bound to CDS -->
                    <Button text="Next" press="onNextStep" />
                </WizardStep>
                
                <!-- Step 5: Final Weighing (Tare/Net Weight) -->
                <WizardStep id="step5" title="Final Weighing" validated="false">
                    <Input value="{TareWeight}" description="{TareWeightUnit}" />
                    <Input value="{NetWeight}" description="{NetWeightUnit}" editable="false" />  <!-- Calculated -->
                    <Button text="Capture Final Weight" press="onCaptureFinalWeight" />
                </WizardStep>
                
                <!-- Step 6: Receive Weighing Slip (Printing) -->
                <WizardStep id="step6" title="Complete and Print" validated="false">
                    <Text text="Session Summary: {SessionId}" />
                    <Button text="Print Slip" press="onPrintSlip" />
                    <Button text="Submit" press="onSubmit" />
                </WizardStep>
            </Wizard>
        </content>
    </Page>
</mvc:View>
