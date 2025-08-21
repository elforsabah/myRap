this.editFlow.invokeAction("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", {
    model: this.getView().getModel(),
    contexts: oContext,
    parameterValues: [{ name: "vbeln", value: sContractId }],
    skipParameterDialog: true
}).then(function (oResult) {
    // Your existing then handler
}).catch(function (oError) {
    // Your existing catch handler
});
