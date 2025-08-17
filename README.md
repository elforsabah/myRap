sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast"
], function (PageController, MessageToast) {
    "use strict";
    return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
        onInit: function () {
            PageController.prototype.onInit.apply(this);
            // Initialize wizard
            this.oWizard = this.byId("weighingWizard");

            // Attach to route pattern matched instead of calling in onInit
            var oRouter = this.getAppComponent().getRouter();
            oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);
        },

        _onObjectMatched: function () {
            var that = this;
            // Create new draft context here to ensure editFlow is initialized
            this.editFlow.createDocument({
                creationMode: "NewPage"  // Or "Inline" depending on your flow
            }).then(function (oNewContext) {
                that.getView().setBindingContext(oNewContext);
                // Pre-request the property to initialize it (avoids "must not change before read")
                oNewContext.requestProperty("Vbeln").catch(function () {
                    // Ignore if not needed; ensures read before any set
                });
            }).catch(function (oError) {
                MessageToast.show("Failed to create session context.");
            });
        },

        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            if (oCurrentStep.getId() === "step1") { // Handle Step 1 validation 
                var sContractId = this.byId("ip11").getValue(); // Get Contract ID (Vbeln) from input 
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
                this.editFlow.invokeAction("identifycard", {
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

        onStepActivate: function (oEvent) {
            // Custom logic per step, e.g., fetch data from CDS
        },
        validateStep: function (oStep) {
            // Add validation logic, e.g., check required fields
            return true;  // Placeholder
        },

        onCaptureWeight: function () {
            // Simulate weighing: Update model with gross weight
            this.getView().getModel().setProperty("/GrossWeight", 1000);  // Replace with actual API call if integrated with hardware
        },

        onCaptureFinalWeight: function () {
            // Calculate net weight: Net = Gross - Tare
            var oModel = this.getView().getModel();
            var fGross = oModel.getProperty("/GrossWeight");
            var fTare = oModel.getProperty("/TareWeight");
            oModel.setProperty("/NetWeight", fGross - fTare);
        },

        onSubmit: function () {
            // Submit to backend via OData create/update
            this.getView().getModel().submitChanges({
                success: function () {
                    MessageToast.show("Weighing session submitted.");
                }
            });
        },

        onPrintSlip: function () {
            // Integrate printing, e.g., via sap.m.Print or external service
        },

        onWizardComplete: function () {
            // Final actions on wizard completion
        }
    });
});
