sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/ui/core/message/Message",
    "sap/ui/core/library",
    "sap/m/Button",
    "sap/ui/model/json/JSONModel",
    "sap/ui/model/resource/ResourceModel",
    "jquery.sap.global"
],
    function (PageController, MessageToast, Messaging, Message, coreLibrary, Button, JSONModel, ResourceModel, jQuery) {
        "use strict";
        return PageController.extend("com.prologa.zwrweighbrigefinal.ext.main.Main", {
            formatter: {
                concatMaterialText: function (sMaterial, sMaterialText) {
                    return (sMaterial || '') + ' - ' + (sMaterialText || '');
                }
            },
            onInit: function () {
                PageController.prototype.onInit.apply(this);
                // Set up i18n model with initial language (English)
                var oResourceModel = new ResourceModel({
                    bundleName: "com.prologa.zwrweighbrige.i18n.i18n",
                    bundleLocale: "en"
                });
                this.getView().setModel(oResourceModel, "@i18n");
                // Messaging
                Messaging.registerObject(this.getView(), true);
                this.getView().setModel(Messaging.getMessageModel(), "message");
                // Attach to message model change to intercept and handle messages
                this.oMessageModel = this.getView().getModel("message");
                this.oMessageModel.attachPropertyChange(this._onMessageChange, this);
                // Wizard
                this.oWizard = this.byId("weighingWizard");
                this.oWizard.attachStepActivate(this.onStepActivate, this);
                // Router
                var oRouter = this.getAppComponent().getRouter();
                oRouter.getRoute("ZI_WR_R_WEIGHBRIDGEMain").attachPatternMatched(this._onObjectMatched, this); // Updated route name based on new entity
                this._enterWired = false; // guard so we wire Enter only once
                this._isFromScan = true;
                // Local model for unbound inputs
                var oLocalModel = new JSONModel({
                    contractId: "",
                    grossWeight: "",
                    teraWeight: "",
                    loadType: "",
                    transaction_state: ""
                });
                this.getView().setModel(oLocalModel, "local");
            },
            getResourceBundle: function () {
                return this.getView().getModel("@i18n").getResourceBundle();
            },
            onSetEnglish: function () {
                var oResourceModel = new ResourceModel({
                    bundleName: "com.prologa.zwrweighbrige.i18n.i18n",
                    bundleLocale: "en"
                });
                this.getView().setModel(oResourceModel, "@i18n");
            },
            onSetDanish: function () {
                var oResourceModel = new ResourceModel({
                    bundleName: "com.prologa.zwrweighbrige.i18n.i18n",
                    bundleLocale: "da"
                });
                this.getView().setModel(oResourceModel, "@i18n");
            },
            _onMessageChange: function (oEvent) {
                var aMessages = this.oMessageModel.getData() || [];
                var aErrors = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Error";
                });
                if (aErrors.length > 0) {
                    // Remove error messages to prevent popup/dialog
                    Messaging.removeMessages(aErrors);
                    // Set inline error using the message text (if related to contract)
                    var sErrorText = aErrors[0].getMessage() || this.getResourceBundle().getText("invalidContract");
                    this._setContractInlineError(sErrorText);
                }
                var aSuccesses = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success";
                });
                if (aSuccesses.length > 0) {
                    // Optionally handle success messages, e.g., show toast and remove
                    MessageToast.show(aSuccesses[0].getMessage());
                    Messaging.removeMessages(aSuccesses);
                }
            },
            _onObjectMatched: function () {
                this.getView().getModel("local").setProperty("/contractId", ""); // Reset local value
                this.getView().getModel("local").setProperty("/grossWeight", ""); // Reset local value
                this.getView().getModel("local").setProperty("/teraWeight", ""); // Reset local value
                this.getView().getModel("local").setProperty("/loadType", ""); // Reset local value
                // Clear step 2 items binding to reset load types
                var oVBox = this.byId("step2LtContainer");
                if (oVBox) {
                    oVBox.unbindAggregation("items");
                    oVBox.destroyItems();
                }
                // Reset wizard to step 1
                this.oWizard.setCurrentStep(this.byId("step1"));
                // Clear any inline errors
                this._clearContractInlineError();
                // Hide confirm button
                var oButton = this.byId("step3BtnConfirm");
                if (oButton) {
                    oButton.setVisible(false);
                }
                var oButton = this.byId("step3BtnConfirm2");
                if (oButton) {
                    oButton.setVisible(true);
                }
            },
            // === ENTER wiring (ADDED) ===
            _setupEnterToNext: function () {
                if (this._enterWired) { return; }
                this._enterWired = true;
                // Step 1: Enter on Contract input -> reuse existing onNextStep
                var oIp = this.byId("step1InputContract");
                if (oIp) {
                    oIp.addEventDelegate({
                        onsapenter: function (oEvent) {
                            oEvent.preventDefault();
                            oEvent.stopPropagation();
                            this.onNextStep();
                        }.bind(this)
                    }, oIp);
                }
            },
            onNextStep: function () {
                var oCurrentStep = this.oWizard.getCurrentStep();
                var sStepId = oCurrentStep.split("--").pop();
                if (sStepId === "step1") {
                    var sContractId = this.getView().getModel("local").getProperty("/contractId");
                    if (!sContractId) {
                        MessageToast.show(this.getResourceBundle().getText("enterContractId"));
                        return;
                    }
                    // Pad with leading zeros for internal format
                    sContractId = sContractId.padStart(10, '0');
                    // Manual bound action call to avoid automatic popups from editFlow
                    var oModel = this.getView().getModel();
                    // Bind action directly with key path (no create/submit needed)
                    console.log("Invoking identifyCard...");
                    var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                    var oAction = oModel.bindContext(sActionPath);
                    oAction.setParameter("Vbeln", sContractId);
                    oAction.invoke().then(function (oResult) {
                        console.log("identifyCard invoked successfully."); // Debug: Action succeeded
                        // Manually create entity context with known key
                        var oContext = oModel.getContext("/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')");
                        this.getView().setBindingContext(oContext);
                        // Refresh context to pull server updates
                        oContext.refresh();
                        // Bind step 2 dynamically
                        var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')/Set"; // 
                        var oVBox = this.byId("step2LtContainer");
                        var oTemplate = new Button({
                            text: {
                                parts: [
                                    { path: 'Avvcode', targetType: 'any' },
                                    { path: 'Material', targetType: 'any' },
                                    { path: 'MaterialText', targetType: 'any' }
                                ],
                                formatter: '.formatter.concatMaterialText'
                            },
                            press: [this.onChooseLoadType, this],
                            width: "100%"
                        });
                        oTemplate.addStyleClass("loadTypeBtn");
                        if (oVBox
