sap.ui.define([
  "sap/fe/core/PageController",
  "sap/m/MessageBox",
  "sap/ui/core/Messaging",
  "sap/ui/model/json/JSONModel"
], function (PageController, MessageBox, Messaging, JSONModel) {
  "use strict";

  return PageController.extend("com.prologa.zwrweighbrigefinal.ext.main.Main", {
    onInit: function () {
      const ret = PageController.prototype.onInit.apply(this);

      // Message manager
      Messaging.registerObject(this.getView(), true);
      this.getView().setModel(Messaging.getMessageModel(), "message");

      // Local view model for user input
      this.getView().setModel(new JSONModel({ contractId: "" }), "view");

      // Route hook (adjust if your route id differs)
      this.getAppComponent().getRouter()
        .getRoute("ZI_WR_WEIGHBRIDGEMain")
        .attachPatternMatched(this._onObjectMatched, this);

      return ret;
    },

    _onObjectMatched: function () {
      const oModel = this.getView().getModel();
      const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");

      // Ensure metadata is ready, then create a draft row
      oModel.requestMetadata().then(() => {
        const oCtx = oListBinding.create({}); // managed numbering creates SESSIONID
        this.getView().setBindingContext(oCtx);
        return oCtx.requestObject();
      }).catch((e) => {
        MessageBox.error(e.message || "Failed to create draft");
      });
    },

    // User typed/scanned the contract and left the field
    onContractChange: function () {
      const oCtx = this.getView().getBindingContext();
      if (!oCtx) { return; }

      const sVbeln = this.getView().getModel("view").getProperty("/contractId") || "";
      if (!sVbeln) { return; }

      // Call bound action identifyCard(<vbeln>)
      oCtx.invokeAction("identifyCard", {
        method: "POST",
        parameters: { vbeln: sVbeln }
      }).then(() => {
        // Refresh fields filled by backend action
        return oCtx.requestSideEffects([
          { $PropertyPath: "SalesDocument" },
          { $PropertyPath: "SalesDocumentType" },
          { $PropertyPath: "SDDocumentCategory" }
        ]);
      }).catch((e) => {
        MessageBox.error(e.message || "identifyCard failed");
      });
    },

    onScanCard: function () {},       // optional stub
    onNextStep: function () {},       // optional stub
    onChooseLoadType: function () {}, // optional stub
    onBackToStep1: function () {}     // optional stub
  });
});
