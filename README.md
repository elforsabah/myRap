onNextStep: function () {
    var oCurrentStep = this.oWizard.getCurrentStep();
    var sStepId = oCurrentStep.split("--").pop();
    if (sStepId === "step1") {
        var sContractId = this.byId("ip11").getValue();
        if (!sContractId) {
            MessageToast.show("Please enter a Contract ID.");
            return;
        }
        var oContext = this.getView().getBindingContext();
        if (!oContext) {
            MessageToast.show("No session context available.");
            return;
        }
        oContext.setProperty("Vbeln", sContractId);  // Still update the property if needed

        // Alternative invocation without EditFlow
        var oModel = this.getView().getModel();  // Get the OData V4 model
        var sActionPath = "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)";  // Adjust namespace if different
        var oActionBinding = oModel.bindContext(sActionPath, oContext);  // Bind the action to the entity context

        // Set parameters (required for your action)
        oActionBinding.setParameter("vbeln", sContractId);
        oActionBinding.setParameter("loadtype", "");  // Empty or default value, as before

        // Invoke the action
        oActionBinding.invoke()
            .then(function (oResult) {
                // On success: oResult may contain the returned entity (your action returns [1] $self)
                MessageToast.show("Contract validated successfully.");
                this.oWizard.validateStep(this.byId(sStepId));
                this.oWizard.nextStep();
                
                // Optional: Refresh bindings if side effects occurred
                oContext.refresh();
            }.bind(this))
            .catch(function (oError) {
                MessageToast.show("Invalid Contract. Please try again.");
                console.error(oError);
            }.bind(this));
    } else {
        this.oWizard.nextStep();
    }
},
