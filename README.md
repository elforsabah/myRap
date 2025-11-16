var oCtx = this.getView().getBindingContext();
    var sIndicator = oCtx ? oCtx.getProperty("Korselsnrindicator") : null;

    this.oWizard.validateStep(this.byId("step2"));

    if (!sIndicator) {
        // Skip stepK entirely by validating it here and advancing twice
        this.oWizard.validateStep(this.byId("stepK"));
        this.oWizard.nextStep(); // To stepK (but it's now complete)
        this.oWizard.nextStep(); // To step3
    } else {
        this.oWizard.nextStep(); // To stepK as normal
    }
