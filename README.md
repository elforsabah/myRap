onNextStep: function () {
  var oView    = this.getView();
  var oModel   = oView.getModel();
  var oWizard  = this.oWizard;
  var oContext = oView.getBindingContext();

  // Read current step (optional; keep if you branch per step)
  var oCurrentStep = oWizard && oWizard.getCurrentStep && oWizard.getCurrentStep();
  var sStepId = oCurrentStep && oCurrentStep.getId ? oCurrentStep.getId().split("--").pop() : "";

  // Example: step 1 expects a contract number
  if (sStepId === "step1") {
    var sContractRaw = this.byId("step1InputContract").getValue();
    if (!sContractRaw) {
      sap.m.MessageToast.show("Please enter a Contract ID.");
      return; // stop here
    }
  }

  // Ensure we have a context; if not, create via FE to keep things consistent
  var pEnsureContext = oContext
    ? Promise.resolve(oContext)
    : this.editFlow.createDocument().then(function (oNewCtx) {
        oView.setBindingContext(oNewCtx);
        return oNewCtx;
      });

  return pEnsureContext
    .then(function (oCtx) {
      // wait until POST finished (transient -> persisted draft)
      return oCtx.created().then(function () { return oCtx; });
    })
    .then(function (oCtx) {
      // acquire RAP draft lock (enter edit mode)
      return this.editFlow.editDocument(oCtx).then(function () { return oCtx; }.bind(this));
    }.bind(this))
    .then(function (oCtx) {
      // Prepare parameters
      var sContractId = this.byId("step1InputContract").getValue().padStart(10, "0");

      // Invoke the bound action on the context
      return this.editFlow.invokeAction(
        "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard",
        {
          model: oModel,
          contexts: [oCtx],
          parameterValues: [{ name: "vbeln", value: sContractId }],
          skipParameterDialog: true
        }
      ).then(function () { return oCtx; });
    }.bind(this))
    .then(function (oCtx) {
      // OPTIONAL: refresh fields impacted by the action (adjust paths to your entity)
      return oCtx.requestSideEffects([{
        $PropertyPath: [
          "Vbeln",             // example
          "LoadType",          // example
          "CardId"             // example
        ]
      }]).catch(function () {
        // Side effects may be skipped if backend doesn’t support them—don’t block UX
      }).then(function () { return oCtx; });
    })
    .then(function () {
      sap.m.MessageToast.show("Card identified.");
      if (oWizard && oWizard.nextStep) {
        oWizard.nextStep();
      }
    })
    .catch(function (oError) {
      // Show a nice error; MessageManager will also have backend details if provided
      var sMsg = (oError && oError.message) || String(oError) || "Unknown error";
      sap.ui.require(["sap/m/MessageBox"], function (MessageBox) {
        MessageBox.error(sMsg);
      });
    });
}
