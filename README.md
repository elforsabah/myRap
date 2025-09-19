sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/m/MessageBox",
    "sap/ui/core/message/Message",
    "sap/ui/core/library",
    "sap/ui/model/Filter",
    "sap/ui/model/FilterOperator",
    "sap/m/Button",
    "sap/ui/model/json/JSONModel",
    "jquery.sap.global"
],
    function (PageController, MessageToast, Messaging, MessageBox, Message, coreLibrary, Filter, FilterOperator, Button, JSONModel, jQuery) {
        "use strict";
        return PageController.extend("com.example.weighingsessionwizard.controller.CustomPage", {
            formatter: {
                concatMaterialText: function (sMaterial, sMaterialText) {
                    return (sMaterial || '') + ' - ' + (sMaterialText || '');
                }
            },
            onInit: function () {
                PageController.prototype.onInit.apply(this);
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
                oRouter.getRoute("ZI_WR_WEIGHINGSESSIONMain").attachPatternMatched(this._onObjectMatched, this);
                this._enterWired = false; // guard so we wire Enter only once
                this._isFromScan = true;
                // Local model for unbound inputs
                this.getView().setModel(new JSONModel({ contractId: "" }), "local");
                this.getView().setModel(new JSONModel({ mainWeight: "" }), "local");
                this.getView().setModel(new JSONModel({ loadType: "" }), "local");
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
                    var sErrorText = aErrors[0].getMessage() || "Invalid Contract. Please try again.";
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
                var oModel = this.getView().getModel();
                var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION", undefined, undefined, undefined, { $$updateGroupId: "weighingGroup" });
                var oNewContext = oListBinding.create({}); // No initial data; let backend manage Sessionid (UUID)
                this.getView().setBindingContext(oNewContext);
                oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () { });
                this.getView().getModel("local").setProperty("/contractId", ""); // Reset local value
                this.getView().getModel("local").setProperty("/mainWeight", ""); // Reset local value
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
                var oContext = this.getView().getBindingContext();
                var oCurrentStep = this.oWizard.getCurrentStep();
                var sStepId = oCurrentStep.split("--").pop();
                if (sStepId === "step1") {
                    var sContractId = this.getView().getModel("local").getProperty("/contractId");
                    if (!sContractId) {
                        MessageToast.show("Please enter a Contract ID.");
                        return;
                    }
                    if (!oContext) {
                        MessageToast.show("No session context available.");
                        return;
                    }
                    // Pad with leading zeros for internal format
                    sContractId = sContractId.padStart(10, '0');
                    // Manual bound action call to avoid automatic popups from editFlow
                    var oModel = this.getView().getModel();
                    // Submit batch to persist the transient entity (create request)
                    oModel.submitBatch("weighingGroup");
                    // Wait for persistence confirmation
                    console.log("Waiting for entity creation..."); // Debug: Start of create chain
                    oContext.created().then(function () {
                        console.log("Entity created successfully. Invoking identifyCard..."); // Debug: Create succeeded
                        var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", oContext);
                        oAction.setParameter("Vbeln", sContractId);
                        return oAction.invoke();
                    }.bind(this)).then(function (oResult) {
                        console.log("identifyCard invoked successfully."); // Debug: Action succeeded
                        // Refresh context to pull server-set Vbeln and any other updates
                        oContext.refresh();
                        // Bind step 2 dynamically
                        var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')/Set"; // Add /Set here
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
                        if (oVBox) {
                            try {
                                oVBox.bindAggregation("items", {
                                    path: sPath,
                                    parameters: {
                                        $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Avvcode,Language",
                                        $orderby: "SalesOrder,SalesOrderitem"
                                    },
                                    template: oTemplate,
                                    templateShareable: false,
                                    events: {
                                        change: function (oEvent) {
                                            if (oEvent.getParameter("reason") === "Rejected") {
                                                MessageToast.show("Failed to load load types. Check contract or service.");
                                                this._skipToWeigh();
                                            }
                                        }.bind(this)
                                    }
                                });
                                // Attach one-time dataReceived to check if empty
                                var oBinding = oVBox.getBinding("items");
                                oBinding.attachEventOnce("dataReceived", function (oEvent) {
                                    if (oBinding.getLength() === 0) {
                                        // No loads, skip step 2
                                        this._skipToWeigh();
                                    }
                                }.bind(this));
                            } catch (oError) {
                                console.error("Binding error: ", oError);
                                this._setContractInlineError("Failed to load materials for contract.");
                                return; // Prevent advancing
                            }
                        } else {
                            console.error("step2LtContainer not found");
                        }
                        // advance wizard
                        this._clearContractInlineError();
                        this.oWizard.validateStep(this.byId("step1"));
                        MessageToast.show("Contract is Valid. Step 2 activated")
                        this.oWizard.nextStep();
                    }.bind(this)).catch(function (oError) {
                        console.error("Error in create or identifyCard: ", oError); // Debug: Unified catch for full error details
                        // Handle failure (original logic consolidated into one catch for simplicity)
                        var sErrorMsg = oError.message || "Unknown error"; // This captures the backend message like "Contract is Invalid"
                        this._setContractInlineError(sErrorMsg); // Display the exact message inline
                    }.bind(this));
                }
            },
            _skipToWeigh: function () {
                var oCtx = this.getView().getBindingContext();
                if (oCtx) {
                    oCtx.setProperty("LoadType", "");
                    this.getView().getModel("local").
