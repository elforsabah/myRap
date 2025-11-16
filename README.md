onChooseLoadType: function (oEvent) {
    const sLoadType = oEvent.getSource().getBindingContext().getProperty("Material");
    const oSessionCtx = this.getView().getBindingContext();
    if (!oSessionCtx) { return; }
    this.getView().getModel("local").setProperty("/loadType", sLoadType);
    this.oWizard.validateStep(this.byId("step2"));

    // Async check for indicator (fetches if not loaded)
    var oCtx = this.getView().getBindingContext();
    oCtx.requestProperty("Korselsnrindicator").then(function (sIndicator) {
        if (!sIndicator) {
            // Skip stepK
            this.oWizard.nextStep(); // Advance to stepK (but validate/skip immediately)
            this.oWizard.validateStep(this.byId("stepK"));
            this.oWizard.nextStep(); // Advance to step3
        } else {
            // Proceed to stepK
            this.oWizard.nextStep();
        }
    }.bind(this)).catch(function (oError) {
        console.error("Error fetching Korselsnrindicator: ", oError);
        // Fallback: Skip or show error (e.g., if property doesn't exist in metadata)
        this.oWizard.nextStep();
        this.oWizard.validateStep(this.byId("stepK"));
        this.oWizard.nextStep();
        MessageToast.show(this.getResourceBundle().getText("failedIndicatorCheck")); // Add i18n key if needed
    }.bind(this));
},


onStepActivate: function (oEvent) {
    var oStep = oEvent.getParameter("step");
    if (!oStep) { return; }
    var sId = oStep.getId().split("--").pop();
    if (sId === "stepK") {
        // No skip check here anymore; only focus if we reach this step
        var oIp = this.byId("stepKInputKorselsnr");
        if (oIp) oIp.focus();
    }
    // NEW: Call onWeighStep3 when entering step3
    if (sId === "step3") {
        this.onWeighStep3();
    }
},
