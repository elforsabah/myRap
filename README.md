 <Page id="page1" title="Terminal App Wizard">
        <content>
            <Wizard id="wizard1">
                <WizardStep id="step1" title="Contract">
                    <form:SimpleForm id="form1" editable="true">
                        <form:content>
                            <Label id="label1" text="Sales Document" />
                            <Input id="input1" value="{/vbeln}" />
                            <Label id="label2" text="Session ID" />
                            <Input id="input2" value="{/sessionid}" />
                        </form:content>
                    </form:SimpleForm>
                </WizardStep>
                <WizardStep id="step2" title="Identification">
                    <form:SimpleForm id="form2" editable="true">
                        <form:content>
                            <Label id="label3" text="Load Type" />
                            <Input id="input3" value="{/loadtype}" />
                            <Label id="label4" text="Gross Weight" />
                            <Input id="input4" value="{/grossweight}" />
                            <Input id="input5" value="{/grossweightunit}" />
                        </form:content>
                    </form:SimpleForm>
                </WizardStep>
                <WizardStep id="step3" title="Weighing">
                    <form:SimpleForm id="form3" editable="true">
                        <form:content>
                            <Label id="label5" text="Net Weight" />
                            <Input id="input6" value="{/netweight}" />
                            <Input id="input7" value="{/netweightunit}" />
                            <Label id="label6" text="Tare Weight" />
                            <Input id="input8" value="{/tareweight}" />
                            <Input id="input9" value="{/tareweightunit}" />
                        </form:content>
                    </form:SimpleForm>
                </WizardStep>
                <WizardStep id="step4" title="Completion">
                    <Button id="button1" text="Submit" press="onSubmit" />
                </WizardStep>
            </Wizard>
        </content>
    </Page>
