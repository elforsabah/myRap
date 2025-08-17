<Page title="Terminal App Wizard">
        <content>
            <Wizard id="terminalWizard">
                <WizardStep title="Contract">
                    <form:SimpleForm editable="true">
                        <form:content>
                            <Label text="Sales Document" />
                            <Input value="{/vbeln}" />
                            <Label text="Session ID" />
                            <Input value="{/sessionid}" />
                        </form:content>
                    </form:SimpleForm>
                </WizardStep>
                <WizardStep title="Identification">
                    <form:SimpleForm editable="true">
                        <form:content>
                            <Label text="Load Type" />
                            <Input value="{/loadtype}" />
                            <Label text="Gross Weight" />
                            <Input value="{/grossweight}" />
                            <Input value="{/grossweightunit}" />
                        </form:content>
                    </form:SimpleForm>
                </WizardStep>
                <WizardStep title="Weighing">
                    <form:SimpleForm editable="true">
                        <form:content>
                            <Label text="Net Weight" />
                            <Input value="{/netweight}" />
                            <Input value="{/netweightunit}" />
                            <Label text="Tare Weight" />
                            <Input value="{/tareweight}" />
                            <Input value="{/tareweightunit}" />
                        </form:content>
                    </form:SimpleForm>
                </WizardStep>
                <WizardStep title="Completion">
                    <Button text="Submit" press="onSubmit" />
                </WizardStep>
            </Wizard>
        </content>
    </Page>
