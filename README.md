
var oModel = this.getView().getModel();
var oOperationBinding = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", oContext, {
    $$ownRequest: true // Ensures a separate request if needed
});
oOperationBinding.setParameter("vbeln", sContractId);
oOperationBinding.invoke().then(function (oResult) {
    // Your existing then handler logic here, e.g., process messages, advance wizard, request side effects
    var aMsgs = Messaging.getMessageModel().getData() || [];
    var aUnboundSuccess = aMsgs.filter(function (oMsg) {
        return oMsg.getTarget && oMsg.getTarget() === "" &&
               oMsg.getType && oMsg.getType() === "Success";
    });
    if (aUnboundSuccess.length > 0) {
        MessageToast.show(aUnboundSuccess[0].getMessage()); // Note: Use .show for MessageToast
    }
    this.oWizard.validateStep(this.byId("step1"));
    this.oWizard.nextStep();
    oContext.requestSideEffects([
        { $PropertyPath: "Vbeln" },
        { $PropertyPath: "CustomerName" }, // Adjust to your fields
        { $PropertyPath: "Step1Ok" }
    ]).catch(function () { /* ignore */ });
}.bind(this)).catch(function (oError) {
    // Your existing catch handler
    console.error("Action Error:", oError);
    MessageBox.error("Invalid Contract. Please try again.");
}.bind(this));
