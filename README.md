
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
            var oView = this.getView();
            var oLocalModel = oView.getModel("local");
            var oModel = oView.getModel();
            var sKorselsnr = oLocalModel.getProperty("/korselsnr");

            // (Optional) Required check + padding
            if (!sKorselsnr) {
                MessageToast.show("Please enter Korselsnr");
                return;
            }
            // If you need 70 chars:
            // sKorselsnr = sKorselsnr.padStart(70, "0");
            // oLocalModel.setProperty("/korselsnr", sKorselsnr);

            var oSessionCtx = oView.getBindingContext();
            if (!oSessionCtx) {
                MessageToast.show("No session context available");
                return;
            }

            // --- 1) Get current path (might include identifyCard(...)) ---
            var sBasePath = oSessionCtx.getPath(); // e.g. "/ZI_WR_R_WEIGHBRIDGE('40000000')/...identifyCard(...)"

            // --- 2) Strip off previous operation if present ---
            var sOpSegment = "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(";
            var iOpIndex = sBasePath.indexOf(sOpSegment);

            if (iOpIndex > -1) {
                // keep only the entity part: "/ZI_WR_R_WEIGHBRIDGE('40000000')"
                sBasePath = sBasePath.slice(0, iOpIndex);
            }

            // --- 3) Now build the correct identifyKorsel path ---
            var sActionPath = sBasePath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyKorsel(...)";

            var oAction = oModel.bindContext(sActionPath);
            oAction.setParameter("Korselsnr", sKorselsnr);

            oAction.execute().then(function () {
                var oResultCtx = oAction.getBoundContext();
                if (oResultCtx) {
                    oView.setBindingContext(oResultCtx);
                }

                // Move wizard to the next step after the action finished
                var oWizard = this.oWizard || this.byId("WeighBridgeWizard");
                var oStepK = this.byId("stepK");

                if (oWizard && oStepK) {
                    oWizard.validateStep(oStepK);   // mark step as completed
                    // If you still have some "weigh" init logic, call it here:
                     this.onWeighStep3();
                    oWizard.nextStep();             // go to next step
                }
            }.bind(this)).catch(function (oError) {
                var sMsg = (oError && oError.message) || "Error calling identifyKorsel";
                MessageToast.show(sMsg);
            }.bind(this));
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

CLASS lhc_Weighingbridge DEFINITION INHERITING FROM cl_abap_behavior_handler.
  PRIVATE SECTION.
    DATA gs_ls_weighing_result TYPE zwr_swaa_selfweigh_result.
    DATA gv_weigh_transaction_started TYPE abap_bool.
    data gv_ewawa_korsel type ewawa_weightext.
    METHODS get_instance_authorizations FOR INSTANCE AUTHORIZATION
      IMPORTING keys REQUEST requested_authorizations FOR Weighingbridge RESULT result.

    METHODS printSlip FOR READ
      IMPORTING keys FOR FUNCTION Weighingbridge~printSlip RESULT result.

    METHODS determineWeight FOR MODIFY
      IMPORTING keys FOR ACTION Weighingbridge~determineWeight RESULT result.

    METHODS identifyCard FOR MODIFY
      IMPORTING keys FOR ACTION Weighingbridge~identifyCard RESULT result.

    METHODS identifyKorsel FOR MODIFY
      IMPORTING keys FOR ACTION Weighingbridge~identifyKorsel RESULT result.

ENDCLASS.

CLASS lhc_Weighingbridge IMPLEMENTATION.

******************************************************************************************
* Authorization
******************************************************************************************
  METHOD get_instance_authorizations.
    READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
      ENTITY Weighingbridge
      FIELDS ( SalesDocument ) WITH CORRESPONDING #( keys )
      RESULT DATA(lt_sessions)
      FAILED failed.

    result = VALUE #( FOR ls_session IN lt_sessions
      ( %tky   = ls_session-%tky
*      %create              = if_abap_behv=>auth-allowed
        %update              = if_abap_behv=>auth-allowed
        %action-identifyCard = if_abap_behv=>auth-allowed
        %action-determineWeight  = if_abap_behv=>auth-allowed ) ).
  ENDMETHOD.

******************************************************************************************
* Identification of Card
******************************************************************************************
  METHOD identifyCard.

    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_r_weighbridge.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
      " Validate contract existence (adjust table name if not VBAK)
*      SELECT SINGLE @abap_true FROM zi_wr_r_weighbridge
*        WHERE SalesDocument = @<ls_key>-%param-vbeln

      SELECT SINGLE @abap_true FROM zi_wr_r_weighbridge
        WHERE IDCardNr = @<ls_key>-%param-Vbeln

        INTO @DATA(lv_exists).
      IF sy-subrc = 0.
        " ✓ Valid: report success message only (no update)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '000'
                   severity = if_abap_behv_message=>severity-success
                   v1 = <ls_key>-%param-vbeln ) )
          TO reported-Weighingbridge.

        " Collect for result
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
      ELSE.
        " ✗ Invalid: report bound error and fail the action
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '001'
                   severity = if_abap_behv_message=>severity-error
                   v1 = <ls_key>-%param-vbeln )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind message to Vbeln field
        ) TO reported-Weighingbridge.

        " Fail the function (set failed to prevent result for this key)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-Weighingbridge.
      ENDIF.

      "***************************************************************************
      "Call and check if there is already a Transaction for this Sales Document
      "**************************************************************************
*      ZCL_WEIGHING_HANDLER=>cl_has_open_weighingproc(
*          EXPORTING
*            iv_contractID   = <ls_key>-%param-vbeln                      "Sales Document
*          RECEIVING
*            RV_HAS_WPROC =  DATA(lv_weigh_transaction_started)        "Weighing Transaction: True if there is and false if there is not
*        ).

    ENDLOOP.

*    IF lv_weigh_transaction_started = abap_true .
*      "When there is already a transaction for this
*      APPEND VALUE #(
*        %tky = <ls_key>-%tky
*        %msg = new_message(
*                 id = 'ZWR_WEIGHBRIGE_MESS'
*                 number = '007'
*                 severity = if_abap_behv_message=>severity-success
*                 v1 = <ls_key>-%param-vbeln ) )
*        TO reported-Weighingbridge.
*
*      " Collect for result
*      APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
*    ENDIF.

    " Read and return full data for successful instances
    IF lt_success_keys IS NOT INITIAL.
      READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
        ENTITY Weighingbridge
        ALL FIELDS WITH lt_success_keys
        RESULT DATA(lt_result)
        FAILED DATA(lt_read_failed).
      result = CORRESPONDING #( lt_result ).
    ENDIF.
  ENDMETHOD.


******************************************************************************************
* Identification of Korsel
******************************************************************************************
  METHOD identifykorsel.


    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_r_weighbridge.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

        SELECT SINGLE * FROM zi_wr_r_weighbridge
        WHERE SalesDocument = @<ls_key>-%key-SalesDocument INTO @data(ls_weighbrigh).

      IF  <ls_key>-%param-Korselsnr is not INITIAL.
        " ✓ Valid: report success message only (no update)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '010'
                   severity = if_abap_behv_message=>severity-success
                   v1 = <ls_key>-%param-Korselsnr ) )
          TO reported-Weighingbridge.
        " Collect for result
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
      ELSE.
*        " ✗ Invalid: report bound error and fail the action
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '011'
*                   severity = if_abap_behv_message=>severity-error
                   severity = if_abap_behv_message=>severity-success
                   v1 = <ls_key>-%param-Korselsnr )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind message to Vbeln field
        ) TO reported-Weighingbridge.

        " Fail the function (set failed to prevent result for this key)
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-Weighingbridge.
      ENDIF.
    ENDLOOP.
    " Read and return full data for successful instances
    IF lt_success_keys IS NOT INITIAL.
      READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
        ENTITY Weighingbridge
        ALL FIELDS WITH lt_success_keys
        RESULT DATA(lt_result)
        FAILED DATA(lt_read_failed).
      result = CORRESPONDING #( lt_result ).
    ENDIF.
  ENDMETHOD.
*********************************************************************************************
* Selection of Load type
*********************************************************************************************
  METHOD determineWeight.
    DATA lt_success_keys TYPE TABLE FOR READ IMPORT zi_wr_r_weighbridge.

    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).
*
TRY.
        ZCL_WR_WEIGHBRIDGE_HELPER=>cl_weighing(
          EXPORTING
            iv_contractid = <ls_key>-%param-Vbeln " Sales Document
            iv_wasteid = <ls_key>-%param-Loadtype "Waste = MATNR
*           * iv_licence_plate = " License plate number
            iv_simulation = abap_false
            iv_user = SY-UNAME " User Name in User Master Record
            iv_commit = abap_false " No need for explicit commit. Commit is handle by RAP framework
          IMPORTING
            iv_weighing_result = gs_ls_weighing_result " returning information from weighing process
        ).
      CATCH cx_eewa_base INTO DATA(lx_error).
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'  " Use your custom message class
                   number = '012'  " Assume a message number for weighing error
                   severity = if_abap_behv_message=>severity-error
                   v1 = lx_error->get_text( ) )
          %element-salesdocument = if_abap_behv=>mk-on  " Bind to relevant field if needed
        ) TO reported-weighingbridge.
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %fail-cause = if_abap_behv=>cause-unspecific
        ) TO failed-weighingbridge.
        CONTINUE.  " Skip further processing for this failed key
      ENDTRY.


      " Read the current session data
      READ ENTITIES OF zi_wr_r_weighbridge IN LOCAL MODE
        ENTITY Weighingbridge
        ALL FIELDS WITH VALUE #( ( %tky = <ls_key>-%tky ) )
        RESULT DATA(lt_sessions)
        FAILED DATA(lt_failed).

      IF lt_sessions IS NOT INITIAL.
        DATA(ls_session) = lt_sessions[ 1 ].
        DATA lv_unit TYPE weight_unit.
        " Example: Assume weight is calculated or fetched (e.g., from a device or table)
        IF gs_ls_weighing_result-bruttoweight IS NOT INITIAL.
          DATA(lv_grossweight) = gs_ls_weighing_result-bruttoweight. "
          lv_unit   = gs_ls_weighing_result-weightunit.
        ENDIF.
        IF gs_ls_weighing_result-taraweight IS NOT INITIAL.
          DATA(lv_teraweight)  = gs_ls_weighing_result-taraweight. "
          lv_unit   = gs_ls_weighing_result-weightunit.
        ENDIF.

        lv_unit = 'KG'.
        IF lv_grossweight IS INITIAL.
          " Simulate fetching weight
          lv_grossweight =  5674367." Example value in kg
*        " Update the entity with the weight
        ENDIF.

        IF lv_teraweight IS INITIAL.
          " Simulate fetching weight
*         lv_teraweight = 3998765. " Example value in kg
*        " Update the entity with the weight
        ENDIF.

        IF lv_grossweight IS NOT INITIAL.
          " Report success of Gross Weight
          APPEND VALUE #(
            %tky = <ls_key>-%tky
            %msg = new_message(
                     id = 'ZWR_WEIGHBRIGE_MESS'
                     number = '002'
                     severity = if_abap_behv_message=>severity-success
                     v1 = |{ lv_grossweight }  { lv_unit }|
                      )
          ) TO reported-weighingbridge.

        ENDIF.

        IF lv_teraweight IS NOT INITIAL.
          " Report success of Tera weight
          APPEND VALUE #(
            %tky = <ls_key>-%tky
            %msg = new_message(
                     id = 'ZWR_WEIGHBRIGE_MESS'
                     number = '009'
                     severity = if_abap_behv_message=>severity-success
                     v1 = |{ lv_teraweight  }  { lv_unit }| )
          ) TO reported-weighingbridge.

        ENDIF.
        " Collect for result
        APPEND VALUE #( %tky = <ls_key>-%tky ) TO lt_success_keys.
      ELSE.
        " Report error if session not found
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '003'
                   severity = if_abap_behv_message=>severity-error
                   v1 = 'Session not found' )
        ) TO reported-weighingbridge.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-not_found ) TO failed-weighingbridge.
      ENDIF.
    ENDLOOP.

  ENDMETHOD.

*********************************************************************************************
* Printing of slip
*********************************************************************************************
  METHOD printslip.
    DATA lv_base64 TYPE string.
    DATA lv_b64 TYPE string.
    DATA lv_b64_v2 TYPE string.
    DATA ls_wa TYPE ewa_wa_weighproc.
    LOOP AT keys ASSIGNING FIELD-SYMBOL(<ls_key>).

      " Fetch the base64 PDF using the helper method
      ls_wa-brutto_weight  = gs_ls_weighing_result-bruttoweight.   "
      ls_wa-tare_weight    = gs_ls_weighing_result-taraweight.

      ls_wa-brutto_weight  = 5674367.
      ls_wa-tara_weight    = 3998765.
      ls_wa-brutto_unit    = 'KG'.
      ls_wa-tara_unit      = 'KG'.
      ls_wa-vbeln          = <ls_key>-%param-Vbeln.
      ls_wa-waste          = <ls_key>-%param-Loadtype.

      zcl_wr_weighbridge_helper=>form_to_base64(
        EXPORTING
          iv_form   = 'IS_U_WA_SF_WEIGHINGPROCESS'
          iv_lang   = sy-langu
*         iv_show   = abap_true
*         iv_pdf    = abap_false
*         iv_b64    = abap_false
*         iv_pdfpth =
*         iv_b64pth =
*         iv_prevln = 200
          Iv_WA     = ls_wa
        RECEIVING
          r_base64  = lv_base64    "--> This is a string
      ).


      IF   lv_base64 IS NOT INITIAL.
        " Report success message
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '004'  " Assume a new message for PDF generation success
                   severity = if_abap_behv_message=>severity-success
                   v1 = 'PDF generated successfully' )
        ) TO reported-weighingbridge.

        " Directly add to result (combine %tky with result entity fields)
        APPEND VALUE #( %tky = <ls_key>-%tky
                        %param = VALUE #( pdfbase64 = lv_base64 ) ) TO result.
      ELSE.
        " Report error if base64 generation failed
        APPEND VALUE #(
          %tky = <ls_key>-%tky
          %msg = new_message(
                   id = 'ZWR_WEIGHBRIGE_MESS'
                   number = '006'
                   severity = if_abap_behv_message=>severity-error
                   v1 = 'Failed to generate PDF' )
        ) TO reported-weighingbridge.
        APPEND VALUE #( %tky = <ls_key>-%tky %fail-cause = if_abap_behv=>cause-unspecific ) TO failed-weighingbridge.
      ENDIF.
    ENDLOOP.
  ENDMETHOD.


ENDCLASS.






