sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",  // Import the Messaging module
    "sap/m/MessageBox"        // Optional for displaying messages

], function (PageController, MessageToast, Messaging, MessageBox) {
    "use strict";
    return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
        onInit: function () {
            var parentReturn = PageController.prototype.onInit.apply(this);
            // Register the view with Messaging (replaces oMessageManager.registerObject)
            Messaging.registerObject(this.getView(), true);

            // Set the message model 
            this.getView().setModel(Messaging.getMessageModel(), "message");

            // Initialize wizard
            this.oWizard = this.byId("weighingWizard");

            // Attach to route pattern matched
            var oRouter = this.getAppComponent().getRouter();
            oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);

            return parentReturn;
        },

        generateUUID: function () {
            // Standard UUID v4 generator for backend compatibility
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        },

        _onObjectMatched: function () {
            var oModel = this.getView().getModel();
            var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
            var oNewContext = oListBinding.create({
                Sessionid: this.generateUUID()  // Generate valid UUID for Sessionid (adjusted case to match CDS)
            });
            this.getView().setBindingContext(oNewContext);
            // Request all bound properties to initialize and include in queries
            oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () {
                // Ignore if needed
            });
        },


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
                //oActionBinding.setParameter("loadtype", "");  // Empty or default value, as before

                // Invoke the action
                oActionBinding.invoke().then(
                    function (oResult) {
                        // Do not clear here; new messages are already in the model from the response

                        // Get messages from Messaging
                        var aMessages = Messaging.getMessageModel().getData();
                        console.log("All Messages:", aMessages);

                        if (aMessages.length > 0) {
                            var oMsg = aMessages[0];  // Assuming first message; iterate if multiple
                            console.log("Message Type:", oMsg.getType(), "Text:", oMsg.getMessage(), "Target:", oMsg.getTarget());

                            var oErrorMsg = aMessages.find(function (oMsg) { return oMsg.getType() === "Error"; });
                            if (oErrorMsg) {
                                MessageBox.error(oErrorMsg.getMessage());
                                return;
                            }

                            // Handle success message (e.g., "Contract is Valid")
                            // Example: Show as toast
                            MessageToast.show(aMessages[0].getMessage());

                            // Continue with your wizard logic
                            this.oWizard.validateStep(this.byId("stepId"));  // Adjust as per your app
                            this.oWizard.nextStep();
                            // Optional: Refresh bindings if side effects occurred
                            oContext.refresh();
                        } else {
                            console.log("No messages Returned.");
                        }
                    }.bind(this),
                    function (oError) {
                        // Handle invocation errors (e.g., network or backend failure)
                        console.error("Action Invoke Error:", oError);
                        MessageToast.show("Invalid Contract. Please try again.");
                    }.bind(this)
                );
            } else {
                //this.oWizard.nextStep();
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
            this.getView().getModel().setProperty("/Grossweight", 1000);  // Adjusted case
        },

        onCaptureFinalWeight: function () {
            // Calculate net weight: Net = Gross - Tare
            var oModel = this.getView().getModel();
            var fGross = oModel.getProperty("/Grossweight");  // Adjusted case
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
