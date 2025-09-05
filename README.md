onWeighStep3: function () {
    var oContext = this.getView().getBindingContext();
    var oModel = this.getView().getModel();
    var oLocalModel = this.getView().getModel("local");

    if (!oContext) {
        MessageToast.show("No session context available.");
        return;
    }

    // Ensure the entity is persisted before calling the action
    oModel.submitBatch("weighingGroup").then(function () {
        console.log("Entity persisted. Invoking determineWeight...");
        // Call the determineWeight action
        var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.determineWeight(...)", oContext);
        var sContractId = this.getView().getModel("local").getProperty("/contractId");
        var sLoadType = this.getView().getModel("local").getProperty("/loadType");
        oAction.setParameter("Vbeln", sContractId);
        oAction.setParameter("Loadtype", sLoadType);

        return oAction.invoke();
    }.bind(this)).then(function (oResult) {
        console.log("determineWeight invoked successfully.");
        // Get the action result and context
        var oData = oResult.getObject();
        var fWeight = oData.Grossweight || 0; // Adjust field name as per your action
        // Update the local model with the weight
        oLocalModel.setProperty("/mainWeight", fWeight.toString());

        // Check for success messages in the reported structure
        var oActionContext = oResult.getContext();
        var aReportedMessages = oActionContext.getObject("reported")?.weighingsession || [];
        var oSuccessMessage = aMessages.find(function (oMsg) {
            return oMsg.messageId === "ZWR_WEIGHBRIGE_MESS" && oMsg.messageNumber === "002" && oMsg.type === "Success";
        });

        if (oSuccessMessage) {
            // Extract the weight from the message's v1 field (included in the message text)
            var sMessageText = oSuccessMessage.message; // e.g., "Weight determined: 1000 kg"
            MessageToast.show(sMessageText); // Display the full message with weight
            // Optionally, parse the weight from the message if needed for other UI updates
            // e.g., var sWeight = sMessageText.match(/(\d+)/)?.[1]; // Extract number if needed
        } else {
            // Fallback: Show weight from action result
            MessageToast.show("Weight determined: " + fWeight + " kg");
        }

        // Refresh the context to sync other fields
        oContext.refresh();
        // Advance to the next step if needed
        this.oWizard.validateStep(this.byId("step3"));
        this.oWizard.nextStep();
    }.bind(this)).catch(function (oError) {
        console.error("Error in determineWeight: ", oError);
        var sErrorMsg = oError.message || "Failed to determine weight.";
        MessageToast.show(sErrorMsg);
    });
}
