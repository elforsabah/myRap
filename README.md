sap.ui.define([
  "sap/fe/core/PageController",
  "sap/m/MessageToast",
  "sap/ui/core/Messaging",
  "sap/m/MessageBox",
  "sap/ui/model/json/JSONModel"
], function (PageController, MessageToast, Messaging, MessageBox, JSONModel) {
  "use strict";

  return PageController.extend("com.prologa.zwrweighbrigefinal.ext.main.Main", {
    onInit: function () {
      const parentReturn = PageController.prototype.onInit.apply(this);

      // Messaging
      Messaging.registerObject(this.getView(), true);
      this.getView().setModel(Messaging.getMessageModel(), "message");

      // Local view model (user input etc.)
      this.getView().setModel(new JSONModel({
        contractId: ""     // user types/scans here
      }), "view");

      this.oWizard = this.byId("weighingWizard");

      // Route
      this.getAppComponent().getRouter()
        .getRoute("ZI_WR_WEIGHBRIDGEMain")
        .attachPatternMatched(this._onObjectMatched, this);

      this._enterWired = false;
      this._isFromScan = true;
      return parentReturn;
    },

    _onObjectMatched: function () {
      const oModel = this.getView().getModel();

      // Bind to entity set and create a new draft instance
      const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");
      const oCtx = oListBinding.create({});   // no unknown or computed props

      // Make it the view context
      this.getView().setBindingContext(oCtx);

      // Optional: ensure data is loaded (instead of requestProperty([...]))
      oCtx.requestObject().catch(() => {});
    },

    // User pressed Enter / left the field: call bound action identifyCard(vbeln)
    onContractChange: function () {
      const oCtx = this.getView().getBindingContext();
      if (!oCtx) { return; }

      const sVbeln = this.getView().getModel("view").getProperty("/contractId") || "";
      if (!sVbeln) { return; }

      // Call bound action: identifyCard
      oCtx.invokeAction("identifyCard", {
        method: "POST",
        parameters: { vbeln: sVbeln } // matches EDM parameter
      }).then(() => {
        // pull updated properties like SalesDocument back from backend
        return oCtx.requestSideEffects([
          { $PropertyPath: "SalesDocument" },
          { $PropertyPath: "SalesDocumentType" },
          { $PropertyPath: "SDDocumentCategory" }
        ]);
      }).catch((e) => {
        MessageBox.error(e.message || "Action failed");
      });
    },

    // (optional stubs so XML handlers don’t explode even if not used yet)
    onScanCard: function () {},
    onNextStep: function () {},
    onChooseLoadType: function () {},
    onBackToStep1: function () {}
  });
});
