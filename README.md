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
            var oContext = this.getView().getBindingContext();
            var svbeln = oContext && oContext.getProperty("Vbeln")
            
            var oCurrentStep = this.oWizard.getCurrentStep();
            var sStepId = oCurrentStep.split("--").pop();
            if (sStepId === "step1") {
                var sContractId = this.byId("ipContract").getValue();
                if (!sContractId) {
                    MessageToast.show("Please enter a Contract ID.");
                    return;
                }
                
                if (!oContext) {
                    MessageToast.show("No session context available.");
                    return;
                }
                oContext.setProperty("Vbeln", sContractId);

                // Use EditFlow for Fiori Elements
                this.editFlow.invokeAction("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", {
                    model: this.getView().getModel(),
                    contexts: oContext,  // Single context for bound action
                    parameterMap: {
                        vbeln: sContractId  // Correct parameter name
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
            } else {
                //this.oWizard.nextStep();
            }
        },

        onAfterRendering: function(){
            var oIp = this.byId("ipContract");
            if (oIp) { oIp.focus(); }
        },
          

        onChooseLoadType: function (oEvent) {
        const sLoadType = oEvent.getSource().getBindingContext().getProperty("LoadType");
        const oSessionCtx = this.getView().getBindingContext();   // bound to ZI_WR_WEIGHINGSESSION
        if (!oSessionCtx) { return; }

        // write the selected value into your session entity
        oSessionCtx.setProperty("LoadType", sLoadType); // field exists in your root entity
        // mark step valid and advance
        this.oWizard.validateStep(this.byId("step2"));
        this.oWizard.nextStep();
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
