<VBox id="ltContainer"
      items="{
        path: '/ZI_WR_LOADTYPEVH',
        parameters: { $select: 'LoadType,LoadTypeText', $orderby: 'LoadType' }
      }">
  <Button
    class="loadTypeBtn"
    width="100%"
    text="{= ${LoadType} + ' - ' + ${LoadTypeText} }"
    press=".onChooseLoadType" />
</VBox>

onChooseLoadType: function (oEvent) {
  const sLoadType = oEvent.getSource().getBindingContext().getProperty("LoadType");
  const oSessionCtx = this.getView().getBindingContext();   // bound to ZI_WR_WEIGHINGSESSION
  if (!oSessionCtx) { return; }

  // write the selected value into your session entity
  oSessionCtx.setProperty("LoadType", sLoadType); // field exists in your root entity
  // mark step valid and advance
  this.oWizard.validateStep(this.byId("step2"));
  this.oWizard.nextStep();
}

.loadTypeBtn {
  margin: 0.5rem 0;
  padding: 1.25rem 1rem;           /* taller button */
  border: 1px solid #d0d5db;       /* subtle outline */
  border-radius: 0.5rem;
  text-align: left;                /* like a card */
  font-weight: 600;
}
