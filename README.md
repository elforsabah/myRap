
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
], function (PageController, MessageToast, Messaging, Message, coreLibrary, Button, JSONModel, ResourceModel, Filter, FilterOperator, jQuery) {
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
                transaction_state: "",
                korselsnr: "",  // NEW: Added for optional Korselsnr step
                idcardnr: ""  // NEW: Added for step 1
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
            this.getView().getModel("local").setProperty("/korselsnr", ""); // NEW: Reset Korselsnr
            this.getView().getModel("local").setProperty("/idcardnr", ""); // NEW: Reset Korselsnr
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
            // NEW: Enter on Korselsnr input -> call onNextFromK
            var oIpK = this.byId("stepKInputKorselsnr");
            if (oIpK) {
                oIpK.addEventDelegate({
                    onsapenter: function (oEvent) {
                        oEvent.preventDefault();
                        oEvent.stopPropagation()
                        this.onNextFromK();
                    }.bind(this)
                }, oIpK);
            }
        },
        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            var sStepId = oCurrentStep.split("--").pop();

            if (sStepId === "step1") {
                // 1) Read ID card number from local model
                var oLocalModel = this.getView().getModel("local");
                var sIdCardNr = oLocalModel.getProperty("/idcardnr"); // here: contractId is the ID card input

                if (!sIdCardNr) {
                    MessageToast.show(this.getResourceBundle().getText("enterContractId"));
                    return;
                }

                // IDCardNr is 9 characters
                sIdCardNr = sIdCardNr.padStart(9, "0");

                var oModel = this.getView().getModel();

                // 2) Find the entity by IDCardNr
                var oListBinding = oModel.bindList(
                    "/ZI_WR_R_WEIGHBRIDGE",
                    undefined,
                    undefined,
                    new Filter("IDCardNr", FilterOperator.EQ, sIdCardNr),
                    { $$ownRequest: true }
                );

                oListBinding.requestContexts(0, 1).then(function (aContexts) {
                    if (aContexts.length > 0) {

                        // 3) We have a match → get the SalesDocument key
                        var oFoundCtx = aContexts[0];
                        var sSalesDocument = oFoundCtx.getProperty("SalesDocument"); // 

                        // Optional: make sure it’s 10 characters (usually it already is)
                        sSalesDocument = sSalesDocument.padStart(10, "0");

                        // Store SalesDocument in local model for all subsequent steps
                        oLocalModel.setProperty("/contractId", sSalesDocument);

                        console.log("Invoking identifyCard with IDCardNr", sIdCardNr, "and SalesDocument", sSalesDocument);

                        // 4) Build action path using the *SalesDocument* key of the entity
                        var sEntityPath = oFoundCtx.getPath(); // e.g. "/ZI_WR_R_WEIGHBRIDGE('0000123456')"
                        var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                        var oAction = oModel.bindContext(sActionPath);

                        // Parameter Vbeln is the ID-card (9 chars)
                        oAction.setParameter("Vbeln", sIdCardNr);

                        oAction.execute().then(function () {
                            console.log("identifyCard invoked successfully.");

                            // 5) Use the returned context from action as your session context
                            var oContext = oAction.getBoundContext();
                            this.getView().setBindingContext(oContext);
                            oContext.refresh();

                            // 6) Load types: use the SalesDocument (contract) here
                            var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sSalesDocument + "')/Set";
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
                                            $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Avvcode,Language,Korselsnrindicator,Korselsnr",
                                            $orderby: "SalesOrder,SalesOrderitem"
                                        },
                                        template: oTemplate,
                                        templateShareable: false,
                                        events: {
                                            change: function (oEvent) {
                                                if (oEvent.getParameter("reason") === "Rejected") {
                                                    MessageToast.show(this.getResourceBundle().getText("failedLoadTypes"));
                                                    this._skipToWeigh();
                                                }
                                            }.bind(this)
                                        }
                                    });

                                    var oBinding = oVBox.getBinding("items");
                                    oBinding.attachEventOnce("dataReceived", function () {
                                        if (oBinding.getLength() === 0) {
                                            this._skipToWeigh();
                                        }
                                    }.bind(this));

                                } catch (oError) {
                                    console.error("Binding error: ", oError);
                                    this._setContractInlineError(this.getResourceBundle().getText("failedMaterials"));
                                    return;
                                }
                            } else {
                                console.error("step2LtContainer not found");
                            }

                            // Messages: same as before
                            var aMessages = Messaging.getMessageModel().getData() || [];
                            var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                                return oMsg.getType() === "Success" && oMsg.getCode() === "ZWR_WEIGHBRIGE_MESS/007";
                            });
                            if (aSpecificSuccesses.length > 0) {
                                oLocalModel.setProperty("/transaction_state", "2. Weighing");
                            } else {
                                oLocalModel.setProperty("/transaction_state", "1. Weighing");
                            }

                            this._clearContractInlineError();
                            this.oWizard.validateStep(this.byId("step1"));
                            MessageToast.show(this.getResourceBundle().getText("contractValid"));
                            this.oWizard.nextStep();

                        }.bind(this)).catch(function (oError) {
                            console.error("Error in identifyCard: ", oError);
                            var sErrorMsg = oError.message || "Unknown error";
                            this._setContractInlineError(sErrorMsg);
                        }.bind(this));

                    } else {
                        // No entity found for this ID card
                        this._setContractInlineError(this.getResourceBundle().getText("contrnotfd"));
                    }
                }.bind(this)).catch(function (oError) {
                    console.error("Entity check error: ", oError);
                    var sErrorMsg = oError.message || "Unknown error";
                    this._setContractInlineError(sErrorMsg);
                }.bind(this));
            }
        },
        _skipToWeigh: function () {
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                this.getView().getModel("local").setProperty("/loadType", "");
            }
            this.oWizard.validateStep(this.byId("step2"));
            // REMOVED: onWeighStep3 call (moved to onStepActivate for step3)
            this.oWizard.nextStep();
        },
        onAfterRendering: function () {
            var oIp = this.byId("step1InputContract");
            if (oIp) { oIp.focus(); }
            this._setupEnterToNext(); // wire Enter once
        },
        onChooseLoadType: function (oEvent) {
            const sLoadType = oEvent.getSource().getBindingContext().getProperty("Material");
            const sIndicator = oEvent.getSource().getBindingContext().getProperty("Korselsnrindicator");
            const sKorselsnr = oEvent.getSource().getBindingContext().getProperty("Korselsnr");
            const oSessionCtx = this.getView().getBindingContext();
            if (!oSessionCtx) { return; }
            this.getView().getModel("local").setProperty("/loadType", sLoadType);
            this.oWizard.validateStep(this.byId("step2"));

            // Async check for indicator (fetches if not loaded)
            if (!sIndicator) {
                // Skip stepK entirely by validating it here and advancing twice
                this.oWizard.validateStep(this.byId("stepK"));
                this.oWizard.nextStep(); // To stepK (but it's now complete)
                this.onWeighStep3();
                this.oWizard.nextStep(); // To step3
            } else {

                this.oWizard.nextStep(); // To stepK as normal
            }
        },
        onStepActivate: function (oEvent) {
            var oStep = oEvent.getParameter("step");
            if (!oStep) { return; }
            var sId = oStep.getId().split("--").pop();
            if (sId === "stepK") {
                // No skip check here anymore; only focus if we reach this step
                var oIp = this.byId("stepKInputKorselsnr");
                if (oIp) oIp.focus();
            }
            // NEW: Call onWeighStep3 when entering step3
            if (sId === "step3") {
                this.onWeighStep3();
            }
        },
        onWeighStep3: function () {
            var oContext = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            var oLocalModel = this.getView().getModel("local");
            if (!oContext) {
                MessageToast.show(this.getResourceBundle().getText("noContext"));
                return;
            }
            console.log("Invoking determineWeight...");
            // Call the determineWeight action
            var sContractId = oLocalModel.getProperty("/contractId").padStart(10, '0');
            var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.determineWeight(...)";
            var oAction = oModel.bindContext(sActionPath);
            var sLoadType = oLocalModel.getProperty("/loadType");
            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            // oAction.invoke().then(function (oResult) {
            oAction.execute().then(function (oResult) {
                console.log("determineWeight invoked successfully.");
                // Get the returned context from action
                var oContext = oAction.getBoundContext();
                this.getView().setBindingContext(oContext);
                // Refresh context for any other updates
                oContext.refresh();
                // NEW: Manually fetch all messages from the message model
                var aMessages = Messaging.getMessageModel().getData() || [];
                // Filter for the specific success message using getCode()
                console.log("All Sucess Messages:", aMessages);
                // Grossweight
                var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/002';
                });
                // Handle Gross Weight
                if (aSpecificSuccesses.length > 0) {
                    // Take the first (or last) matching message; assuming one per action
                    var oSpecificMsg = aSpecificSuccesses[0]; // or aSpecificSuccesses[aSpecificSuccesses.length - 1]
                    var sMsgText = oSpecificMsg.getMessage();
                    oLocalModel.setProperty("/grossWeight", sMsgText);
                    // Remove the processed specific success messages to avoid accumulation or popups
                    Messaging.removeMessages(aSpecificSuccesses);
                } else {
                    MessageToast.show(this.getResourceBundle().getText("noSuccess002"));
                }
                // Teraweight
                var aSpecificSuccesses1 = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/009';
                });
                console.log("Mess 009:", aSpecificSuccesses1);
                // Handle TeratWeight
                if (aSpecificSuccesses1.length > 0) {
                    // Take the first (or last) matching message; assuming one per action
                    var oSpecificMsg = aSpecificSuccesses1[0]; // or aSpecificSuccesses[aSpecificSuccesses.length - 1]
                    var sMsgText1 = oSpecificMsg.getMessage();
                    oLocalModel.setProperty("/teraWeight", sMsgText1);
                    // Make the CONFIRM button visible after weight is set
                    var oButton = this.byId("step3BtnConfirm");
                    if (oButton) {
                        oButton.setVisible(true);
                    }
                    var oButton = this.byId("step3BtnConfirm2");
                    if (oButton) {
                        oButton.setVisible(false);
                    }
                    // Remove the processed specific success messages to avoid accumulation or popups
                    Messaging.removeMessages(aSpecificSuccesses1);
                } else {
                    //MessageToast.show("No specific success message (ZWR_WEIGHBRIGE_MESS/009) received from backend.");
                }
                var sGrossweight1 = oLocalModel.getProperty("/grossWeight");
                var steraweight1 = oLocalModel.getProperty("/teraWeight");
                console.log("grossweight:", sGrossweight1);
                console.log("teraweight:", steraweight1);
                MessageToast.show(this.getResourceBundle().getText("weightCaptured") + sGrossweight1 + "\n" + steraweight1, { duration: 5000 });
                jQuery(".sapMMessageToast").addClass("myGreenToast");
            }.bind(this)).catch(function (oError) {
                console.error("Error in determineWeight: ", oError);
                var sErrorMsg = oError.message || this.getResourceBundle().getText("failedWeight");
                MessageToast.show(sErrorMsg);
                // Optional: Also check for error messages and handle/remove them
                var aMessages = Messaging.getMessageModel().getData() || [];
                var aErrors = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Error";
                });
                if (aErrors.length > 0) {
                    // Example: Show the error message and remove
                    MessageToast.show(aErrors[0].getMessage());
                    Messaging.removeMessages(aErrors);
                }
            }.bind(this));
        },
        // NEW: Handler for next from optional Korselsnr step (analogous to onNextStep)
        onNextFromK: function () {
            var sKorselsnr = this.getView().getModel("local").getProperty("/korselsnr");
            if (!sKorselsnr) {
                MessageToast.show(this.getResourceBundle().getText("stepKLabel")); // add new i18n
                return;
            }
            // Pad with leading zeros for internal format "Korselsnr"
            sKorselsnr = sKorselsnr.padStart(70, '0');
            // Manual bound action call
            var oModel = this.getView().getModel();
            // Check if entity exists
            var oListBinding = oModel.bindList("/ZI_WR_R_WEIGHBRIDGE", undefined, undefined, new Filter("Korselsnr", FilterOperator.EQ, sKorselsnr), { $$ownRequest: true });
            oListBinding.requestContexts(0, 1).then(function (aContexts) {
                if (aContexts.length > 0) {
                    // Entity exists, get the SalesDocument key
                    var sSalesDocument = aContexts[0].getProperty("SalesDocument");
                    // Proceed with action using the key
                    console.log("Invoking identifyKorsel for Korselsnr...");
                    var sEntityPath = "/ZI_WR_R_WEIGHBRIDGE('" + sSalesDocument + "')";
                    var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyKorsel(...)";
                    // Pad with leading zeros for internal format "Korselsnr"
                    var oAction = oModel.bindContext(sActionPath);
                    oAction.setParameter("Korselsnr", sKorselsnr); // Use Korselsnr as input
                    // oAction.invoke().then(function (oResult) {
                    oAction.execute().then(function (oResult) {
                        console.log("identifyKorsel invoked successfully for Korselsnr.");
                        // Get the returned context from action
                        var oContext = oAction.getBoundContext();
                        this.getView().setBindingContext(oContext);
                        // Refresh context
                        oContext.refresh();
                        // NEW: Update local contractId to the SalesDocument (for subsequent actions)
                        this.getView().getModel("local").setProperty("/contractId", sSalesDocument);
                        // Manually fetch messages (similar to step1)
                        var aMessages = Messaging.getMessageModel().getData() || [];
                        var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                            return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/007';
                        });
                        if (aSpecificSuccesses.length > 0) {
                            this.getView().getModel("local").setProperty("/transaction_state", "2. Weighing");
                        } else {
                            this.getView().getModel("local").setProperty("/transaction_state", "1. Weighing");
                        }
                        // Advance wizard
                        this._clearKorselsnrInlineError();
                        this.oWizard.validateStep(this.byId("stepK"));
                        MessageToast.show(this.getResourceBundle().getText("contractValid")); // Reuse or add new
                        this.onWeighStep3();
                        this.oWizard.nextStep();
                    }.bind(this)).catch(function (oError) {
                        console.error("Error in identifyCard for Korselsnr: ", oError);
                        var sErrorMsg = oError.message || "Unknown error";
                        this._setKorselsnrInlineError(sErrorMsg);
                    }.bind(this));
                } else {
                    this._setKorselsnrInlineError(this.getResourceBundle().getText("korselnotfd"));
                }
            }.bind(this)).catch(function (oError) {
                console.error("Entity check error for Korselsnr: ", oError);
                var sErrorMsg = oError.message || "Unknown error";
                this._setKorselsnrInlineError(sErrorMsg);
            }.bind(this));
        },
        // NEW: Inline error setters for Korselsnr (analogous to contract)
        _setKorselsnrInlineError: function (sText) {
            var oInput = this.byId("stepKInputKorselsnr");
            if (oInput) {
                oInput.setValueState("Error");
                oInput.setValueStateText(sText);
                oInput.focus();
            }
            // Add field-bound message
            var oCtx = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            if (oCtx && oModel) {
                var sTarget = oCtx.getPath() + "/Korselsnr"; // Use Korselsnr property
                var aAll = Messaging.getMessageModel().getData() || [];
                var aOldForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aOldForField.length) { Messaging.removeMessages(aOldForField); }
                Messaging.addMessages(new Message({
                    message: sText,
                    type: coreLibrary.MessageType.Error,
                    target: sTarget,
                    processor: oModel
                }));
            }
        },
        _clearKorselsnrInlineError: function () {
            var oInput = this.byId("stepKInputKorselsnr");
            if (oInput) {
                oInput.setValueState("None");
                oInput.setValueStateText("");
            }
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                var sTarget = oCtx.getPath() + "/Korselsnr"; // Use Korselsnr property
                var aAll = Messaging.getMessageModel().getData() || [];
                var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aForField.length) { Messaging.removeMessages(aForField); }
            }
        },
        _normalizeToStdBase64: function (val) {
            // If backend already gave binary (Uint8Array/ArrayBuffer), return as-is
            if (val instanceof Uint8Array) return { kind: "u8", data: val };
            if (val && val.buffer instanceof ArrayBuffer) return { kind: "u8", data: new Uint8Array(val) };
            var s = String(val || "").trim();
            // Strip any data URL prefix like "data:application/pdf;base64,..."
            s = s.replace(/^data:[^;]+;base64,/, "");
            // Remove whitespace/newlines
            s = s.replace(/\s+/g, "");
            // Convert base64url -> base64
            s = s.replace(/-/g, "+").replace(/_/g, "/");
            // Pad with '=' to make length % 4 === 0 (without using repeat)
            var pad = s.length % 4;
            if (pad) {
                s += Array(4 - pad + 1).join("=");
            }
            return { kind: "b64", data: s };
        },
        onConfirmStep3: function () {
            var oContext = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            var oLocalModel = this.getView().getModel("local");
            if (!oContext) {
                MessageToast.show(this.getResourceBundle().getText("noContext"));
                return;
            }
            var sContractId = oLocalModel.getProperty("/contractId");
            var sLoadType = oLocalModel.getProperty("/loadType");
            var sGrossWeight = oLocalModel.getProperty("/grossWeight");
            var sTeraWeight = oLocalModel.getProperty("/teraWeight");
            if (!sContractId || !sLoadType || !sGrossWeight) {
                MessageToast.show(this.getResourceBundle().getText("missingData"));
                return;
            }
            // Parse "12345 KG" (fallback unit KG)
            var aWeightParts = sGrossWeight.trim().split(/\s+/);
            var sWeight = aWeightParts[0] || "";
            var sWeighUnit = aWeightParts[1] || "KG";
            // Pad contract like in step 1
            sContractId = sContractId.padStart(10, "0");
            // Bind the action using entity path
            var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.printSlip(...)";
            var oAction = oModel.bindContext(sActionPath);
            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            oAction.setParameter("Weight", sWeight);
            oAction.setParameter("WeighUnit", sWeighUnit);
            // Now call the silent print function
            if (sTeraWeight) { // Print only when there is Tera Weight
                // oAction.invoke().then(function () {
                oAction.execute().then(function () {
                    // Get the returned context from action
                    var oNewContext = oAction.getBoundContext();
                    this.getView().setBindingContext(oNewContext);
                    // Read the result payload from the bound context
                    var oResCtx = oAction.getBoundContext();
                    var oRes = oResCtx && oResCtx.getObject() || null;
                    // Updated to match new metadata (PDFBASE64 uppercase); support variants for robustness
                    var sB64 = oRes && (oRes.PDFBASE64 || oRes.pdfbase64 || oRes.Pdfbase64);
                    if (typeof sB64 === "string") {
                        sB64 = String(sB64);
                    }
                    if (sB64 && typeof sB64 === "string") {
                        try {
                            // Normalize the base64 string first (using your existing _normalizeToStdBase64 for robustness)
                            var norm = this._normalizeToStdBase64(sB64);
                            if (norm.kind === "b64") {
                                sB64 = norm.data; // Use the normalized base64 string
                            } else if (norm.kind === "u8") {
                                // If it's already binary, convert to base64 for _printBase64PdfSilently (which expects base64)
                                sB64 = btoa(String.fromCharCode.apply(null, norm.data));
                            }
                            this._printBase64PdfSilently(sB64); // Print only when there is Teraweight
                            MessageToast.show(this.getResourceBundle().getText("pdfSent"));
                        } catch (oError) {
                            console.error("Base64 processing or printing failed: ", oError);
                            MessageToast.show(this.getResourceBundle().getText("failedPdf"));
                        }
                    } else {
                        MessageToast.show(this.getResourceBundle().getText("noPdf"));
                    }
                    // Optional: surface server messages (Success)
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    console.log("All Messages 3", aMsgs);
                    var aSucc = aMsgs.filter(function (m) {
                        return m.getType && m.getType() === "Success";
                    });
                    if (aSucc.length > 0) {
                        MessageToast.show(aSucc[0].getMessage());
                        Messaging.removeMessages(aSucc);
                    }
                    // Refresh & reset to step 1 instead of next step
                    oNewContext.refresh();
                    this._onObjectMatched();
                }.bind(this)).catch(function (oError) {
                    var sErr = (oError && oError.message) || this.getResourceBundle().getText("failedPdf");
                    MessageToast.show(sErr);
                    // Optional: surface server messages (Error)
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    var aErrs = aMsgs.filter(function (m) {
                        return m.getType && m.getType() === "Error";
                    });
                    if (aErrs.length > 0) {
                        MessageToast.show(aErrs[0].getMessage());
                        Messaging.removeMessages(aErrs);
                    }
                }.bind(this));
            } else {
                // Refresh & reset to step 1 instead of next step
                oContext.refresh();
                this._onObjectMatched();
            }
        },
        /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
        //// Printing in Kiosk Mode for Chrome 
        /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
        _printBase64PdfSilently: function (sB64) {
            // Ensure it's a string (some backends return ArrayBuffer/Uint8Array)
            if (sB64 && sB64.constructor !== String) {
                try { sB64 = atob(btoa(String.fromCharCode.apply(null, new Uint8Array(sB64)))); }
                catch (e) { sB64 = typeof sB64.toString === "function" ? sB64.toString() : String(sB64); }
            }
            // Create a Blob URL (more reliable than giant data-URIs)
            var byteChars = atob(sB64);
            var byteNums = new Array(byteChars.length);
            for (var i = 0; i < byteChars.length; i++) byteNums[i] = byteChars.charCodeAt(i);
            var blob = new Blob([new Uint8Array(byteNums)], { type: "application/pdf" });
            var url = URL.createObjectURL(blob);
            // Hidden iframe
            var iframe = document.createElement("iframe");
            iframe.style.position = "absolute";
            iframe.style.left = "-10000px";
            iframe.style.top = "-10000px";
            iframe.style.width = "0px";
            iframe.style.height = "0px";
            iframe.style.border = "0";
            iframe.style.visibility = "hidden";
            iframe.onload = function () {
                // Important: call print on the iframe's *contentWindow*
                setTimeout(function () {
                    try {
                        iframe.contentWindow.focus();
                        iframe.contentWindow.print(); // dialog-less if Chrome has --kiosk-printing
                    } finally {
                        // Cleanup after a short delay to avoid killing the print job
                        setTimeout(function () {
                            URL.revokeObjectURL(url);
                            iframe.parentNode && iframe.parentNode.removeChild(iframe);
                        }, 1000);
                    }
                }, 200); // give the PDF plugin a moment to render
            };
            iframe.src = url;
            document.body.appendChild(iframe);
        },
        _setContractInlineError: function (sText) {
            // Inline on the input
            var oInput = this.byId("step1InputContract");
            if (oInput) {
                oInput.setValueState("Error");
                oInput.setValueStateText(sText);
                oInput.focus();
            }
            // Optional: also add a field-bound message (shows in FE message popover / keeps state on rebind)
            var oCtx = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            if (oCtx && oModel) {
                var sTarget = oCtx.getPath() + "/SalesDocument"; // Updated to SalesDocument
                var aAll = Messaging.getMessageModel().getData() || [];
                var aOldForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aOldForField.length) { Messaging.removeMessages(aOldForField); }
                Messaging.addMessages(new Message({
                    message: sText,
                    type: coreLibrary.MessageType.Error,
                    target: sTarget,
                    processor: oModel
                }));
            }
        },
        _clearContractInlineError: function () {
            var oInput = this.byId("step1InputContract");
            if (oInput) {
                oInput.setValueState("None");
                oInput.setValueStateText("");
            }
            var oCtx = this.getView().getBindingContext();
            if (oCtx) {
                var sTarget = oCtx.getPath() + "/SalesDocument"; // Updated to SalesDocument
                var aAll = Messaging.getMessageModel().getData() || [];
                var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                if (aForField.length) { Messaging.removeMessages(aForField); }
            }
        },
        onCancel: function () {
            // Reset the wizard and clear data
            this._onObjectMatched();
        }
    });
});
