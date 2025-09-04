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
        return oAction.invoke();
    }.bind(this)).then(function (oResult) {
        console.log("determineWeight invoked successfully.");
        // Assuming the action returns the weight in a field (e.g., Grossweight)
        // Adjust the field name based on your action's result structure
        var oData = oResult.getObject();
        var fWeight = oData.Grossweight || 0; // Adjust field name as per your action
        // Update the local model
        oLocalModel.setProperty("/mainWeight", fWeight.toString());
        // Optionally refresh the context to sync other fields
        oContext.refresh();
        MessageToast.show("Weight determined: " + fWeight);
    }.bind(this)).catch(function (oError) {
        console.error("Error in determineWeight: ", oError);
        var sErrorMsg = oError.message || "Failed to determine weight.";
        MessageToast.show(sErrorMsg);
    });
}
