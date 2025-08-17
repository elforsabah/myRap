onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            if (oCurrentStep.getId() === "step1") {  // Handle Step 1 validation
                var sContractId = this.byId("ip11").getValue();  // Get Contract ID (Vbeln) from input
                if (!sContractId) {
                    MessageToast.show("Please enter a Contract ID.");
                    return;
                }

                var oContext = this.getView().getBindingContext();  // Get current entity context
                if (!oContext) {
                    MessageToast.show("No session context available.");
                    return;
                }

                // Update the model with the input value (if not already bound)
                oContext.setProperty("Vbeln", sContractId);

                // Invoke the RAP action 'identifycard' with parameter (adjust action name/namespace if needed)
                this.extensionAPI.getEditFlow().invokeAction("identifycard", {
                    contexts: [oContext],
                    parameterValues: [{ name: "CardId", value: sContractId }]  // Array of {name, value} objects
                }).then(function () {
                    // On success: Show message and advance
                    MessageToast.show("Contract validated successfully.");
                    this.oWizard.validateStep(oCurrentStep);  // Mark step as valid
                    this.oWizard.nextStep();
                }.bind(this)).catch(function (oError) {
                    // On error: Show message and stay
                    MessageToast.show("Invalid Contract. Please try again.");
                    // Optional: Log error details - console.error(oError);
                }.bind(this));
            } else {
                // For other steps: Simply advance (add logic as needed)
                this.oWizard.nextStep();
            }
        },
