// In onNextStep, before oAction.invoke()
var oEntityContext = oModel.createKey("/ZI_WR_R_WEIGHBRIDGE", { SalesDocument: sContractId });
oModel.requestCanonicalUrl(oModel.getServiceUrl(), oEntityContext).then(function(sUrl) {
    // Proceed with action if entity exists; else handle as invalid
}).catch(function() {
    this._setContractInlineError("Contract not found");
}.bind(this));
