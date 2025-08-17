<!-- Step 3: Initiate Weighing (Gross Weight) -->
<WizardStep id="step3" title="Weighing" validated="false">
  <Input id="ip2" value="{path: 'GrossWeight', targetType: 'any'}" description="{path: 'GrossWeightUnit', targetType: 'any'}" />
  <!-- ... -->
</WizardStep> 

<!-- Step 4: Receive Weighing Slip (Printing) -->
<WizardStep id="step6" title="Complete and Print" validated="false">
  <Text id="tx1" text="Session Summary: {path: 'SessionId', targetType: 'any'}" />
  <!-- ... -->
</WizardStep>
