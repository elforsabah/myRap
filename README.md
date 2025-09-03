onInit: function () {
  this.getView().setModel(new sap.ui.model.json.JSONModel({
    vbeln: "", vbelnState: "None", vbelnStateText: ""
  }), "view");
  this.getAppComponent().getRouter()
    .getRoute("ZI_WR_WEIGHBRIDGEMain")
    .attachPatternMatched(this._onObjectMatched, this);
},

_onObjectMatched: async function () {
  const oListBinding = this.getView().getModel().bindList("/ZI_WR_WEIGHBRIDGE");
  const oCtx = await this.editFlow.createDocument(oListBinding); // draft create
  this.getView().setBindingContext(oCtx);
},

onContractChange: async function () {
  const oViewModel = this.getView().getModel("view");
  const sVbeln = (oViewModel.getProperty("/vbeln") || "").trim().toUpperCase();

  if (!sVbeln) { oViewModel.setProperty("/vbelnState", "Error");
                 oViewModel.setProperty("/vbelnStateText", "Please enter a VBELN"); return; }

  try {
    await this.editFlow.invokeAction(
      "com.sap.gateway.srvd.zsb_wr_weighingbrige1.v0001.identifyCard",
      { contexts: [this.getView().getBindingContext()],
        parameters: { vbeln: sVbeln }, invocationGrouping: "Isolated", label: "Identify Sales Document" }
    );
    await this.editFlow.requestSideEffects([
      { $PropertyPath: "SalesDocument" },
      { $PropertyPath: "SalesDocumentType" },
      { $PropertyPath: "SDDocumentCategory" }
    ], this.getView().getBindingContext());
    oViewModel.setProperty("/vbelnState", "None");
    oViewModel.setProperty("/vbelnStateText", "");
  } catch (e) {
    oViewModel.setProperty("/vbelnState", "Error");
    oViewModel.setProperty("/vbelnStateText", e.message || "identifyCard failed");
    sap.m.MessageBox.error(e.message || "identifyCard failed");
  }
}
