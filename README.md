sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast"
], function (PageController, MessageToast) {
    "use strict";
    return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
        onInit: function () {
            var parentReturn = PageController.prototype.onInit.apply(this);

            // Initialize wizard
            this.oWizard = this.byId("weighingWizard");

            // Attach to route pattern matched
            var oRouter = this.getAppComponent().getRouter();
            oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);

            return parentReturn;
        },

        generateUUID: function () {
            // Standard UUID v4 generator for backend compatibility
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
                var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        },

        _onObjectMatched: function () {
            var oModel = this.getView().getModel();
            var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
            var oNewContext = oListBinding.create({
                SessionId: this.generateUUID()  // Generate valid UUID for SessionId (adjusted case to match CDS)
            });
            this.getView().setBindingContext(oNewContext);
            // Request all bound properties to initialize and include in queries
            oNewContext.requestProperty(["Vbeln", "GrossWeight", "GrossWeightUnit", "SessionId"]).catch(function () {
                // Ignore if needed
            });
        },

        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();  // Returns string ID, e.g., "step1"
            if (oCurrentStep === "step1") { // Compare string directly (fix here)
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
                    parameterValues: [{ name: "Vbeln", value: sContractId }]  // Array of {name, value} objects
                }).then(function () {
                    // On success: Show message and advance
                    MessageToast.show("Contract validated successfully.");
                    this.oWizard.validateStep(this.byId(oCurrentStep));  // Pass step object by ID (fix here)
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
            this.getView().getModel().setProperty("/GrossWeight", 1000);  // Adjusted case
        },

        onCaptureFinalWeight: function () {
            // Calculate net weight: Net = Gross - Tare
            var oModel = this.getView().getModel();
            var fGross = oModel.getProperty("/GrossWeight");  // Adjusted case
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
