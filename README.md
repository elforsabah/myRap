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
        oContext.setProperty("Vbeln", sContractId);

        // Use EditFlow for Fiori Elements
        this.editFlow.invokeAction("identifyCard", {
            model: this.getView().getModel(),
            contexts: oContext,  // Single context for bound action
            parameterMap: {
                CardId: sContractId  // Correct parameter name
            }
        }).then(function (oResult) {
            // Messages should now be in model automatically
            var aMessages = Messaging.getMessageModel().getData();
            console.log("All Messages:", aMessages);

            // Handle unbound success (filter if needed)
            var aUnboundSuccess = aMessages.filter(function (oMsg) {
                return oMsg.getTarget() === "" && oMsg.getType() === "Success";
            });
            if (aUnboundSuccess.length > 0) {
                MessageToast.show(aUnboundSuccess[0].getMessage());
            }

            // Bound messages from result
            if (oResult && oResult.context && oResult.context.getMessages) {
                var aBoundMessages = oResult.context.getMessages();
                if (aBoundMessages.length > 0) {
                    console.log("Bound Messages:", aBoundMessages);
                }
            }

            // Proceed
            this.oWizard.validateStep(this.byId("step1"));  // Use actual ID
            this.oWizard.nextStep();
            oContext.refresh();  // Refresh for side effects like step1ok
        }.bind(this)).catch(function (oError) {
            console.error("Action Error:", oError);
            MessageToast.show("Invalid Contract. Please try again.");
        });
    }
},
