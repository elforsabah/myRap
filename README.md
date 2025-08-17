onNextStep: async function () {
            const oCurrentStep = this.oWizard.getCurrentStep();
            if (oCurrentStep.getId() === "step1") {  // Handle Step 1 validation
                const sContractId = this.byId("ip11").getValue();  // Get Contract ID (Vbeln) from input
                if (!sContractId) {
                    MessageToast.show("Please enter a Contract ID.");
                    return;
                }

                const oContext = this.getView().getBindingContext();  // Get current entity context
                if (!oContext) {
                    MessageToast.show("No session context available.");
                    return;
                }

                // Update the model with the input value (if not already bound)
                oContext.setProperty("Vbeln", sContractId);

                try {
                    // Invoke the RAP action 'identifycard' with parameter (adjust action name/namespace if needed)
                    await this.extensionAPI.getEditFlow().invokeAction("identifycard", {
                        contexts: [oContext],
                        parameterValues: new Map([["CardId", sContractId]])  // Map to backend param; change 'CardId' to 'Vbeln' if applicable
                    });

                    // On success: Show message and advance
                    MessageToast.show("Contract validated successfully.");
                    this.oWizard.validateStep(oCurrentStep);  // Mark step as valid
                    this.oWizard.nextStep();
                } catch (oError) {
                    // On error: Show message and stay
                    MessageToast.show("Invalid Contract. Please try again.");
                    // Optional: Log error details - console.error(oError);
                }
            } else {
                // For other steps: Simply advance (add logic as needed)
                this.oWizard.nextStep();
            }
        },
