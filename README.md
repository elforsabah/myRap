<mvc:View xmlns:core="sap.ui.core" xmlns:mvc="sap.ui.core.mvc" xmlns="sap.m" xmlns:macros="sap.fe.macros" xmlns:f="sap.ui.layout.form"
xmlns:html="http://www.w3.org/1999/xhtml" controllerName="com.prologa.zwrweighbrige.ext.main.Main"
height="100%">
<Page id="Main" class="myApp" >
<content>
<Wizard id="weighingWizard" complete="onWizardComplete" >
<!-- Step 1: Identification (as in screenshot 3) -->
<WizardStep id="step1" title="Identification" validated="false" activate="onStepActivate">
<f:Form id="f1">
<f:layout>
<f:ResponsiveGridLayout id="r1"/>
</f:layout>
<f:formContainers>
<f:FormContainer id="fc1">
<f:formElements>
<f:FormElement label="Please Enter your Contract ID" id="fe1" class="myCustomFormElement" >
<f:fields>
<Input id="ip11" value="{Vbeln}" width="12rem" maxLength="15" /> <!-- Bound to CDS view field -->
</f:fields>
</f:FormElement>
<!-- Add ID Card scan or other identification fields -->
</f:formElements>
</f:FormContainer>
</f:formContainers>
</f:Form>
<Button id="btn1" text="Next" press="onNextStep" />
</WizardStep>
<!-- Step 2: Position Vehicle and Select Load Type -->
<WizardStep id="step2" title="Choose Load Type" validated="false" >
<VBox id="vb1" >
<Label id="lb1" text="Position vehicle on weighbridge" />
<!-- <Label id="hg" text="Load Type" /> -->
<HBox id="das" width="10rem" justifyContent="Center">
<macros:Field metaPath="LoadType" id="loadTypeField" readOnly="false" />
</HBox>
<Button id="btn2" text="Next" press="onNextStep" />
</VBox>
</WizardStep>

<!-- Step 3: Initiate Weighing (Gross Weight) -->
<WizardStep id="step3" title="Weighing" validated="false">
  <Input id="ip2" value="{path: 'Grossweight', targetType: 'any'}" description="{path: 'Grossweightunit', targetType: 'any'}" />
  <!-- Use Fiori Elements Building Block for quantity if needed: <macros:Field definition="{path: 'Grossweight'}" /> -->
  <Button id="btn3" text="Capture Weight" press="onCaptureWeight" />
</WizardStep> 

<!-- Step 4: Receive Weighing Slip (Printing) -->
<WizardStep id="step6" title="Complete and Print" validated="false">
  <Text id="tx1" text="Session Summary: {path: 'Sessionid', targetType: 'any'}" />
  <Button id="btn6" text="Print Slip" press="onPrintSlip" />
  <Button id="btn7" text="Submit" press="onSubmit" />
</WizardStep>

</Wizard>
</content>
</Page>
</mvc:View>
