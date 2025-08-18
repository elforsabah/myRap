// Register MessageManager
    var oMessageManager = sap.ui.getCore().getMessageManager();
    oMessageManager.registerObject(this.getView(), true);  // Attach to the view to auto-collect messages

    // Set message model for later access
    this.getView().setModel(oMessageManager.getMessageModel(), "message");



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

        // Clear any previous messages
        sap.ui.getCore().getMessageManager().removeAllMessages();

        // Invoke action (using EditFlow or direct model)
        this.editFlow.invokeAction("identifyCard", {
            contexts: [oContext],
            parameterValues: [
                { name: "vbeln", value: sContractId },
                { name: "loadtype", value: "" }
            ]
        }).then(function (oResult) {
            // Log entity data if needed
            console.log("Action Result Entity:", oResult.getObject());

            // Handle messages from MessageManager (no direct SAP__Messages access needed)
            var oMessageModel = this.getView().getModel("message");
            var aMessages = oMessageModel.getData();  // Array of messages
            console.log("All Messages:", aMessages);

            if (aMessages.length > 0) {
                aMessages.forEach(function (oMsg) {
                    console.log("Message Type:", oMsg.getType(), "Text:", oMsg.getMessage(), "Target:", oMsg.getTarget());
                });
                // Display errors (e.g., show first error)
                var oErrorMsg = aMessages.find(function (oMsg) { return oMsg.getType() === sap.ui.core.MessageType.Error; });
                if (oErrorMsg) {
                    MessageBox.error(oErrorMsg.getMessage());
                    return;  // Stop if error
                }
            } else {
                console.log("No messages returned.");
            }

            MessageToast.show("Contract validated successfully.");
            this.oWizard.validateStep(this.byId(sStepId));
            this.oWizard.nextStep();
        }.bind(this)).catch(function (oError) {
            // Handle invocation errors (e.g., network or invalid action)
            console.error("Action Invocation Error:", oError);

            // Check MessageManager for details
            var aMessages = this.getView().getModel("message").getData();
            if (aMessages.length > 0) {
                MessageBox.error(aMessages[0].getMessage());  // Example: Show first message
            } else {
                MessageToast.show("Invalid Contract. Please try again.");
            }
        }.bind(this));
    } else {
        this.oWizard.nextStep();
    }
},
