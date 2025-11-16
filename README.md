sap.ui.define([
    "sap/fe/core/PageController",
    "sap/m/MessageToast",
    "sap/ui/core/Messaging",
    "sap/ui/core/message/Message",
    "sap/ui/core/library",
    "sap/m/Button",
    "sap/ui/model/json/JSONModel",
    "sap/ui/model/resource/ResourceModel",
    "sap/ui/model/Filter",
    "sap/ui/model/FilterOperator",
    "jquery.sap.global"
], function (
    PageController,
    MessageToast,
    Messaging,
    Message,
    coreLibrary,
    Button,
    JSONModel,
    ResourceModel,
    Filter,
    FilterOperator,
    jQuery
) {
    "use strict";

    return PageController.extend("com.prologa.zwrweighbrigefinal.ext.main.Main", {
        formatter: {
            concatMaterialText: function (sMaterial, sMaterialText) {
                return (sMaterial || "") + " - " + (sMaterialText || "");
            }
        },

        onInit: function () {
            PageController.prototype.onInit.apply(this);

            // Set up i18n model with initial language (English)
            var oResourceModel = new ResourceModel({
                bundleName: "com.prologa.zwrweighbrigefinal.i18n.i18n",
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

            // Reference to optional stepK (visible=false at start via XML)
            this._oStepK = this.byId("stepK");

            // Router
            var oRouter = this.getAppComponent().getRouter();
            oRouter
                .getRoute("ZI_WR_R_WEIGHBRIDGEMain")
                .attachPatternMatched(this._onObjectMatched, this); // Updated route name based on new entity

            this._enterWired = false; // guard so we wire Enter only once
            this._isFromScan = true;

            // Local model for unbound inputs
            var oLocalModel = new JSONModel({
                contractId: "",
                grossWeight: "",
                teraWeight: "",
                loadType: "",
                transaction_state: "",
                korselsnr: "" // NEW: Added for optional Korselsnr step
            });
            this.getView().setModel(oLocalModel, "local");
        },

        getResourceBundle: function () {
            return this.getView().getModel("@i18n").getResourceBundle();
        },

        onSetEnglish: function () {
            var oResourceModel = new ResourceModel({
                bundleName: "com.prologa.zwrweighbrigefinal.i18n.i18n",
                bundleLocale: "en"
            });
            this.getView().setModel(oResourceModel, "@i18n");
        },

        onSetDanish: function () {
            var oResourceModel = new ResourceModel({
                bundleName: "com.prologa.zwrweighbrigefinal.i18n.i18n",
                bundleLocale: "da"
            });
            this.getView().setModel(oResourceModel, "@i18n");
        },

        _onMessageChange: function () {
            var aMessages = this.oMessageModel.getData() || [];

            var aErrors = aMessages.filter(function (oMsg) {
                return oMsg.getType() === "Error";
            });
            if (aErrors.length > 0) {
                // Remove error messages to prevent popup/dialog
                Messaging.removeMessages(aErrors);
                // Set inline error using the message text (if related to contract)
                var sErrorText =
                    aErrors[0].getMessage() ||
                    this.getResourceBundle().getText("invalidContract");
                this._setContractInlineError(sErrorText);
            }

            var aSuccesses = aMessages.filter(function (oMsg) {
                return oMsg.getType() === "Success";
            });
            if (aSuccesses.length > 0) {
                MessageToast.show(aSuccesses[0].getMessage());
                Messaging.removeMessages(aSuccesses);
            }
        },

        _onObjectMatched: function () {
            var oLocal = this.getView().getModel("local");
            oLocal.setProperty("/contractId", "");
            oLocal.setProperty("/grossWeight", "");
            oLocal.setProperty("/teraWeight", "");
            oLocal.setProperty("/loadType", "");
            oLocal.setProperty("/korselsnr", "");

            // Clear step 2 items binding to reset load types
            var oVBox = this.byId("step2LtContainer");
            if (oVBox) {
                oVBox.unbindAggregation("items");
                oVBox.destroyItems();
            }

            // Reset wizard to step 1
            this.oWizard.setCurrentStep(this.byId("step1"));

            // Hide optional stepK at the beginning of each transaction
            if (this._oStepK) {
                this._oStepK.setVisible(false);
            }

            // Clear any inline errors
            this._clearContractInlineError();

            // Hide/Show confirm buttons
            var oButton = this.byId("step3BtnConfirm");
            if (oButton) {
                oButton.setVisible(false);
            }
            oButton = this.byId("step3BtnConfirm2");
            if (oButton) {
                oButton.setVisible(true);
            }
        },

        // === ENTER wiring ===
        _setupEnterToNext: function () {
            if (this._enterWired) {
                return;
            }
            this._enterWired = true;

            // Step 1: Enter on Contract input -> reuse existing onNextStep
            var oIp = this.byId("step1InputContract");
            if (oIp) {
                oIp.addEventDelegate(
                    {
                        onsapenter: function (oEvent) {
                            oEvent.preventDefault();
                            oEvent.stopPropagation();
                            this.onNextStep();
                        }.bind(this)
                    },
                    oIp
                );
            }

            // NEW: Enter on Korselsnr input -> call onNextFromK
            var oIpK = this.byId("stepKInputKorselsnr");
            if (oIpK) {
                oIpK.addEventDelegate(
                    {
                        onsapenter: function (oEvent) {
                            oEvent.preventDefault();
                            oEvent.stopPropagation();
                            this.onNextFromK();
                        }.bind(this)
                    },
                    oIpK
                );
            }
        },

        // Helper: show / hide stepK based on Korselsnrindicator
        _updateKorselsnrStepVisibility: function () {
            if (!this._oStepK) {
                return;
            }
            var oCtx = this.getView().getBindingContext();
            var sIndicator = oCtx && oCtx.getProperty("Korselsnrindicator");
            var bShow =
                !!sIndicator && String(sIndicator).trim() !== "";
            this._oStepK.setVisible(bShow);
        },

        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            var sStepId = oCurrentStep.split("--").pop();

            if (sStepId === "step1") {
                var oLocal = this.getView().getModel("local");
                var sContractId = oLocal.getProperty("/contractId");
                if (!sContractId) {
                    MessageToast.show(
                        this.getResourceBundle().getText("enterContractId")
                    );
                    return;
                }

                // Pad with leading zeros for internal format
                sContractId = sContractId.padStart(10, "0");

                var oModel = this.getView().getModel();

                // Check if entity exists before invoking action
                var oListBinding = oModel.bindList(
                    "/ZI_WR_R_WEIGHBRIDGE",
                    undefined,
                    undefined,
                    new Filter("SalesDocument", FilterOperator.EQ, sContractId),
                    { $$ownRequest: true }
                );

                oListBinding
                    .requestContexts(0, 1)
                    .then(
                        function (aContexts) {
                            if (aContexts.length > 0) {
                                console.log("Invoking identifyCard...");
                                var sEntityPath =
                                    "/ZI_WR_R_WEIGHBRIDGE('" +
                                    sContractId +
                                    "')";
                                var sActionPath =
                                    sEntityPath +
                                    "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                                var oAction =
                                    oModel.bindContext(sActionPath);
                                oAction.setParameter("Vbeln", sContractId);
                                oAction
                                    .invoke()
                                    .then(
                                        function () {
                                            console.log(
                                                "identifyCard invoked successfully."
                                            );

                                            // Get the returned context from action
                                            var oContext =
                                                oAction.getBoundContext();
                                            this.getView().setBindingContext(
                                                oContext
                                            );
                                            // Refresh context to pull server updates
                                            oContext.refresh();

                                            // Decide whether to show optional stepK
                                            this._updateKorselsnrStepVisibility();

                                            // Bind step 2 dynamically
                                            var sPath =
                                                "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" +
                                                sContractId +
                                                "')/Set";
                                            var oVBox =
                                                this.byId(
                                                    "step2LtContainer"
                                                );
                                            var oTemplate = new Button({
                                                text: {
                                                    parts: [
                                                        {
                                                            path: "Avvcode",
                                                            targetType: "any"
                                                        },
                                                        {
                                                            path: "Material",
                                                            targetType: "any"
                                                        },
                                                        {
                                                            path: "MaterialText",
                                                            targetType: "any"
                                                        }
                                                    ],
                                                    formatter:
                                                        ".formatter.concatMaterialText"
                                                },
                                                press: [
                                                    this.onChooseLoadType,
                                                    this
                                                ],
                                                width: "100%"
                                            });
                                            oTemplate.addStyleClass(
                                                "loadTypeBtn"
                                            );

                                            if (oVBox) {
                                                try {
                                                    oVBox.bindAggregation(
                                                        "items",
                                                        {
                                                            path: sPath,
                                                            parameters: {
                                                                $select:
                                                                    "SalesOrder,SalesOrderitem,Material,MaterialText,Avvcode,Language",
                                                                $orderby:
                                                                    "SalesOrder,SalesOrderitem"
                                                            },
                                                            template: oTemplate,
                                                            templateShareable: false,
                                                            events: {
                                                                change: function (
                                                                    oEvent
                                                                ) {
                                                                    if (
                                                                        oEvent.getParameter(
                                                                            "reason"
                                                                        ) ===
                                                                        "Rejected"
                                                                    ) {
                                                                        MessageToast.show(
                                                                            this.getResourceBundle().getText(
                                                                                "failedLoadTypes"
                                                                            )
                                                                        );
                                                                        this._skipToWeigh();
                                                                    }
                                                                }.bind(this)
                                                            }
                                                        }
                                                    );

                                                    // Attach one-time dataReceived to check if empty
                                                    var oBinding =
                                                        oVBox.getBinding(
                                                            "items"
                                                        );
                                                    oBinding.attachEventOnce(
                                                        "dataReceived",
                                                        function () {
                                                            if (
                                                                oBinding.getLength() ===
                                                                0
                                                            ) {
                                                                // No loads, skip step 2
                                                                this._skipToWeigh();
                                                            }
                                                        }.bind(this)
                                                    );
                                                } catch (oError) {
                                                    console.error(
                                                        "Binding error: ",
                                                        oError
                                                    );
                                                    this._setContractInlineError(
                                                        this.getResourceBundle().getText(
                                                            "failedMaterials"
                                                        )
                                                    );
                                                    return; // Prevent advancing
                                                }
                                            } else {
                                                console.error(
                                                    "step2LtContainer not found"
                                                );
                                            }

                                            //Manually fetch all messages from the message model
                                            var aMessages =
                                                Messaging.getMessageModel().getData() ||
                                                [];
                                            // Filter for the specific success message using getCode()
                                            var aSpecificSuccesses =
                                                aMessages.filter(function (
                                                    oMsg
                                                ) {
                                                    return (
                                                        oMsg.getType() ===
                                                            "Success" &&
                                                        oMsg.getCode() ===
                                                            "ZWR_WEIGHBRIGE_MESS/007"
                                                    );
                                                });
                                            if (
                                                aSpecificSuccesses.length > 0
                                            ) {
                                                oLocal.setProperty(
                                                    "/transaction_state",
                                                    "2. Weighing"
                                                );
                                            } else {
                                                oLocal.setProperty(
                                                    "/transaction_state",
                                                    "1. Weighing"
                                                );
                                            }

                                            // advance wizard
                                            this._clearContractInlineError();
                                            this.oWizard.validateStep(
                                                this.byId("step1")
                                            );
                                            MessageToast.show(
                                                this.getResourceBundle().getText(
                                                    "contractValid"
                                                )
                                            );
                                            this.oWizard.nextStep();
                                        }.bind(this)
                                    )
                                    .catch(
                                        function (oError) {
                                            console.error(
                                                "Error in identifyCard: ",
                                                oError
                                            );
                                            var sErrorMsg =
                                                oError.message ||
                                                "Unknown error";
                                            this._setContractInlineError(
                                                sErrorMsg
                                            );
                                        }.bind(this)
                                    );
                            } else {
                                this._setContractInlineError(
                                    this.getResourceBundle().getText(
                                        "contrnotfd"
                                    )
                                );
                            }
                        }.bind(this)
                    )
                    .catch(
                        function (oError) {
                            console.error(
                                "Entity check error: ",
                                oError
                            );
                            var sErrorMsg =
                                oError.message || "Unknown error";
                            this._setContractInlineError(sErrorMsg);
                        }.bind(this)
                    );
            }
        },

        _skipToWeigh: function () {
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                this.getView()
                    .getModel("local")
                    .setProperty("/loadType", "");
            }
            this.oWizard.validateStep(this.byId("step2"));
            this.oWizard.nextStep();
        },

        onAfterRendering: function () {
            var oIp = this.byId("step1InputContract");
            if (oIp) {
                oIp.focus();
            }
            this._setupEnterToNext();
        },

        onChooseLoadType: function (oEvent) {
            var sLoadType =
                oEvent.getSource().getBindingContext().getProperty("Material");
            var oSessionCtx = this.getView().getBindingContext();
            if (!oSessionCtx) {
                return;
            }
            this.getView()
                .getModel("local")
                .setProperty("/loadType", sLoadType);
            this.oWizard.validateStep(this.byId("step2"));
            this.oWizard.nextStep();
        },

        onStepActivate: function (oEvent) {
            var oStep = oEvent.getParameter("step");
            if (!oStep) {
                return;
            }
            var sId = oStep.getId().split("--").pop();

            // Optional stepK: only usable if indicator is set; otherwise skip
            if (sId === "stepK") {
                var oCtx = this.getView().getBindingContext();
                var sIndicator =
                    oCtx && oCtx.getProperty("Korselsnrindicator");
                if (!sIndicator) {
                    // Safety: if somehow activated without indicator, skip
                    this.oWizard.validateStep(oStep);
                    this.oWizard.nextStep();
                    return;
                }
                var oIp = this
