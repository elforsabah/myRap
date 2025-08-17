<!-- Step 2: Position Vehicle and Select Load Type -->
<WizardStep id="step2" title="Choose Load Type" validated="false">
<VBox id="vb1">
<Label id="lb1" text="Position vehicle on weighbridge" />
<Label text="Load Type" />
<HBox width="10rem" justifyContent="Start">
<macros:Field metaPath="LoadType" id="loadTypeField" readOnly="false" />
</HBox>
</VBox>
<Button id="btn2" text="Next" press="onNextStep" />
</WizardStep>
