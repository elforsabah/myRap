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
            oRouter.getRoute("ZI_WR_R_WEIGHBRIDGEMain").attachPatternMatched(this._onObjectMatched, this);

            this._enterWired = false;
            this._isFromScan = true;

            // Local model for unbound inputs (including flag for optional stepK)
            var oLocalModel = new JSONModel({
                contractId: "",
                grossWeight: "",
                teraWeight: "",
                loadType: "",
                transaction_state: "",
                korselsnr: "",
                showStepK: false // controls visibility of stepK
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
                Messaging.removeMessages(aErrors);
                var sErrorText = aErrors[0].getMessage() || this.getResourceBundle().getText("invalidContract");
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
            oLocal.setProperty("/transaction_state", "");
            oLocal.setProperty("/showStepK", false); // hide optional step for new session

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
            this._clearKorselsnrInlineError();

            // Hide confirm&print; show simple confirm
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
            if (this._enterWired) { return; }
            this._enterWired = true;

            // Enter on Contract input -> onNextStep
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

            // Enter on Korselsnr input -> onNextFromK
            var oIpK = this.byId("stepKInputKorselsnr");
            if (oIpK) {
                oIpK.addEventDelegate({
                    onsapenter: function (oEvent) {
                        oEvent.preventDefault();
                        oEvent.stopPropagation();
                        this.onNextFromK();
                    }.bind(this)
                }, oIpK);
            }
        },

        onNextStep: function () {
            var oCurrentStep = this.oWizard.getCurrentStep();
            var sStepId = oCurrentStep.split("--").pop();

            if (sStepId === "step1") {
                var oLocal = this.getView().getModel("local");
                var sContractId = oLocal.getProperty("/contractId");
                if (!sContractId) {
                    MessageToast.show(this.getResourceBundle().getText("enterContractId"));
                    return;
                }

                // Pad with leading zeros for internal format
                sContractId = sContractId.padStart(10, '0');

                var oModel = this.getView().getModel();

                // Check if entity exists before invoking action
                var oListBinding = oModel.bindList(
                    "/ZI_WR_R_WEIGHBRIDGE",
                    undefined,
                    undefined,
                    new Filter("SalesDocument", FilterOperator.EQ, sContractId),
                    { $$ownRequest: true }
                );

                oListBinding.requestContexts(0, 1).then(function (aContexts) {
                    if (aContexts.length > 0) {
                        // Entity exists, proceed with action
                        console.log("Invoking identifyCard...");
                        var sEntityPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')";
                        var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                        var oAction = oModel.bindContext(sActionPath);
                        oAction.setParameter("Vbeln", sContractId);
                        oAction.invoke().then(function () {
                            console.log("identifyCard invoked successfully.");

                            // Get the returned context from action
                            var oContext = oAction.getBoundContext();
                            this.getView().setBindingContext(oContext);
                            // Refresh context to pull server updates
                            oContext.refresh();

                            // Decide if optional stepK should be visible
                            var sIndicator = oContext.getProperty("Korselsnrindicator");
                            var bShowStepK = !!(sIndicator && String(sIndicator).trim() !== "");
                            this.getView().getModel("local").setProperty("/showStepK", bShowStepK);

                            // Bind step 2 dynamically
                            var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')/Set";
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

                            // Message-based transaction state
                            var aMessages = Messaging.getMessageModel().getData() || [];
                            var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                                return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/007';
                            });
                            if (aSpecificSuccesses.length > 0) {
                                oLocal.setProperty("/transaction_state", "2. Weighing");
                            } else {
                                oLocal.setProperty("/transaction_state", "1. Weighing");
                            }

                            // advance wizard
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
            this.oWizard.nextStep();
        },

        onAfterRendering: function () {
            var oIp = this.byId("step1InputContract");
            if (oIp) { oIp.focus(); }
            this._setupEnterToNext();
        },

        onChooseLoadType: function (oEvent) {
            var sLoadType = oEvent.getSource().getBindingContext().getProperty("Material");
            var oSessionCtx = this.getView().getBindingContext();
            if (!oSessionCtx) { return; }

            this.getView().getModel("local").setProperty("/loadType", sLoadType);
            this.oWizard.validateStep(this.byId("step2"));
            this.oWizard.nextStep(); // goes either to stepK (if visible) or directly to step3
        },

        onStepActivate: function (oEvent) {
            var oStep = oEvent.getParameter("step");
            if (!oStep) { return; }
            var sId = oStep.getId().split("--").pop();

            if (sId === "stepK") {
                var oIp = this.byId("stepKInputKorselsnr");
                if (oIp) { oIp.focus(); }
            }

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

            var sContractId = oLocalModel.getProperty("/contractId").padStart(10, '0');
            var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.determineWeight(...)";
            var oAction = oModel.bindContext(sActionPath);
            var sLoadType = oLocalModel.getProperty("/loadType");

            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            oAction.invoke().then(function () {
                console.log("determineWeight invoked successfully.");

                var oCtx = oAction.getBoundContext();
                this.getView().setBindingContext(oCtx);
                oCtx.refresh();

                var aMessages = Messaging.getMessageModel().getData() || [];
                console.log("All Sucess Messages:", aMessages);

                // Gross Weight
                var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/002';
                });
                if (aSpecificSuccesses.length > 0) {
                    var oSpecificMsg = aSpecificSuccesses[0];
                    var sMsgText = oSpecificMsg.getMessage();
                    oLocalModel.setProperty("/grossWeight", sMsgText);
                    Messaging.removeMessages(aSpecificSuccesses);
                } else {
                    MessageToast.show(this.getResourceBundle().getText("noSuccess002"));
                }

                // Tare Weight
                var aSpecificSuccesses1 = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/009';
                });
                console.log("Mess 009:", aSpecificSuccesses1);
                if (aSpecificSuccesses1.length > 0) {
                    var oSpecificMsg1 = aSpecificSuccesses1[0];
                    var sMsgText1 = oSpecificMsg1.getMessage();
                    oLocalModel.setProperty("/teraWeight", sMsgText1);

                    var oButton = this.byId("step3BtnConfirm");
                    if (oButton) {
                        oButton.setVisible(true);
                    }
                    oButton = this.byId("step3BtnConfirm2");
                    if (oButton) {
                        oButton.setVisible(false);
                    }

                    Messaging.removeMessages(aSpecificSuccesses1);
                }

                var sGrossweight1 = oLocalModel.getProperty("/grossWeight");
                var sTeraweight1 = oLocalModel.getProperty("/teraWeight");
                console.log("grossweight:", sGrossweight1);
                console.log("teraweight:", sTeraweight1);

                MessageToast.show(
                    this.getResourceBundle().getText("weightCaptured") + sGrossweight1 + "\n" + sTeraweight1,
                    { duration: 5000 }
                );
                jQuery(".sapMMessageToast").addClass("myGreenToast");
            }.bind(this)).catch(function (oError) {
                console.error("Error in determineWeight: ", oError);
                var sErrorMsg = oError.message || this.getResourceBundle().getText("failedWeight");
                MessageToast.show(sErrorMsg);

                var aMessages = Messaging.getMessageModel().getData() || [];
                var aErrors = aMessages.filter(function (oMsg) {
                    return oMsg.getType() === "Error";
                });
                if (aErrors.length > 0) {
                    MessageToast.show(aErrors[0].getMessage());
                    Messaging.removeMessages(aErrors);
                }
            }.bind(this));
        },

        // --- Next from optional Korselsnr step ---
        onNextFromK: function () {
            var oLocal = this.getView().getModel("local");
            var sKorselsnr = oLocal.getProperty("/korselsnr");
            if (!sKorselsnr) {
                MessageToast.show(this.getResourceBundle().getText("enterContractId")); // or new i18n
                return;
            }

            sKorselsnr = sKorselsnr.padStart(10, '0');

            var oModel = this.getView().getModel();
            var oListBinding = oModel.bindList(
                "/ZI_WR_R_WEIGHBRIDGE",
                undefined,
                undefined,
                new Filter("Korselsnr", FilterOperator.EQ, sKorselsnr),
                { $$ownRequest: true }
            );

            oListBinding.requestContexts(0, 1).then(function (aContexts) {
                if (aContexts.length > 0) {
                    var sSalesDocument = aContexts[0].getProperty("SalesDocument");

                    console.log("Invoking identifyCard for Korselsnr...");
                    var sEntityPath = "/ZI_WR_R_WEIGHBRIDGE('" + sSalesDocument + "')";
                    var sActionPath = sEntityPath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(...)";
                    var oAction = oModel.bindContext(sActionPath);
                    oAction.setParameter("Vbeln", sKorselsnr);

                    oAction.invoke().then(function () {
                        console.log("identifyCard invoked successfully for Korselsnr.");

                        var oContext = oAction.getBoundContext();
                        this.getView().setBindingContext(oContext);
                        oContext.refresh();

                        // StepK is already visible if this handler is reachable

                        // Update local contractId to SalesDocument
                        oLocal.setProperty("/contractId", sSalesDocument);

                        var aMessages = Messaging.getMessageModel().getData() || [];
                        var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                            return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/007';
                        });
                        if (aSpecificSuccesses.length > 0) {
                            oLocal.setProperty("/transaction_state", "2. Weighing");
                        } else {
                            oLocal.setProperty("/transaction_state", "1. Weighing");
                        }

                        this._clearKorselsnrInlineError();
                        this.oWizard.validateStep(this.byId("stepK"));
                        MessageToast.show(this.getResourceBundle().getText("contractValid"));
                        this.oWizard.nextStep();
                    }.bind(this)).catch(function (oError) {
                        console.error("Error in identifyCard for Korselsnr: ", oError);
                        var sErrorMsg = oError.message || "Unknown error";
                        this._setKorselsnrInlineError(sErrorMsg);
                    }.bind(this));
                } else {
                    this._setKorselsnrInlineError(this.getResourceBundle().getText("contrnotfd"));
                }
            }.bind(this)).catch(function (oError) {
                console.error("Entity check error for Korselsnr: ", oError);
                var sErrorMsg = oError.message || "Unknown error";
                this._setKorselsnrInlineError(sErrorMsg);
            }.bind(this));
        },

        _setKorselsnrInlineError: function (sText) {
            var oInput = this.byId("stepKInputKorselsnr");
            if (oInput) {
                oInput.setValueState("Error");
                oInput.setValueStateText(sText);
                oInput.focus();
            }

            var oCtx = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            if (oCtx && oModel) {
                var sTarget = oCtx.getPath() + "/Korselsnr";
                var aAll = Messaging.getMessageModel().getData() || [];
                var aOldForField = aAll.filter(function (m) {
                    return m.getTarget && m.getTarget() === sTarget;
                });
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
                var sTarget = oCtx.getPath() + "/Korselsnr";
                var aAll = Messaging.getMessageModel().getData() || [];
                var aForField = aAll.filter(function (m) {
                    return m.getTarget && m.getTarget() === sTarget;
                });
                if (aForField.length) { Messaging.removeMessages(aForField); }
            }
        },

        _normalizeToStdBase64: function (val) {
            if (val instanceof Uint8Array) return { kind: "u8", data: val };
            if (val && val.buffer instanceof ArrayBuffer) return { kind: "u8", data: new Uint8Array(val) };

            var s = String(val || "").trim();
            s = s.replace(/^data:[^;]+;base64,/, "");
            s = s.replace(/\s+/g, "");
            s = s.replace(/-/g, "+").replace(/_/g, "/");
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

            var aWeightParts = sGrossWeight.trim().split(/\s+/);
            var sWeight = aWeightParts[0] || "";
            var sWeighUnit = aWeightParts[1] || "KG";

            sContractId = sContractId.padStart(10, "0");

            var sActionPath = "/ZI_WR_R_WEIGHBRIDGE('" + sContractId + "')/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.printSlip(...)";
            var oAction = oModel.bindContext(sActionPath);
            oAction.setParameter("Vbeln", sContractId);
            oAction.setParameter("Loadtype", sLoadType);
            oAction.setParameter("Weight", sWeight);
            oAction.setParameter("WeighUnit", sWeighUnit);

            if (sTeraWeight) {
                oAction.invoke().then(function () {
                    var oNewContext = oAction.getBoundContext();
                    this.getView().setBindingContext(oNewContext);

                    var oResCtx = oAction.getBoundContext();
                    var oRes = oResCtx && oResCtx.getObject() || null;

                    var sB64 = oRes && (oRes.PDFBASE64 || oRes.pdfbase64 || oRes.Pdfbase64);
                    if (typeof sB64 === "string") {
                        sB64 = String(sB64);
                    }

                    if (sB64 && typeof sB64 === "string") {
                        try {
                            var norm = this._normalizeToStdBase64(sB64);
                            if (norm.kind === "b64") {
                                sB64 = norm.data;
                            } else if (norm.kind === "u8") {
                                sB64 = btoa(String.fromCharCode.apply(null, norm.data));
                            }
                            this._printBase64PdfSilently(sB64);
                            MessageToast.show(this.getResourceBundle().getText("pdfSent"));
                        } catch (oError) {
                            console.error("Base64 processing or printing failed: ", oError);
                            MessageToast.show(this.getResourceBundle().getText("failedPdf"));
                        }
                    } else {
                        MessageToast.show(this.getResourceBundle().getText("noPdf"));
                    }

                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    console.log("All Messages 3", aMsgs);
                    var aSucc = aMsgs.filter(function (m) {
                        return m.getType && m.getType() === "Success";
                    });
                    if (aSucc.length > 0) {
                        MessageToast.show(aSucc[0].getMessage());
                        Messaging.removeMessages(aSucc);
                    }

                    oNewContext.refresh();
                    this._onObjectMatched();
                }.bind(this)).catch(function (oError) {
                    var sErr = (oError && oError.message) || this.getResourceBundle().getText("failedPdf");
                    MessageToast.show(sErr);

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
                oContext.refresh();
                this._onObjectMatched();
            }
        },

        // Printing in Kiosk Mode for Chrome 
        _printBase64PdfSilently: function (sB64) {
            if (sB64 && sB64.constructor !== String) {
                try {
                    sB64 = atob(btoa(String.fromCharCode.apply(null, new Uint8Array(sB64))));
                } catch (e) {
                    sB64 = typeof sB64.toString === "function" ? sB64.toString() : String(sB64);
                }
            }

            var byteChars = atob(sB64);
            var byteNums = new Array(byteChars.length);
            for (var i = 0; i < byteChars.length; i++) {
                byteNums[i] = byteChars.charCodeAt(i);
            }
            var blob = new Blob([new Uint8Array(byteNums)], { type: "application/pdf" });
            var url = URL.createObjectURL(blob);

            var iframe = document.createElement("iframe");
            iframe.style.position = "absolute";
            iframe.style.left = "-10000px";
            iframe.style.top = "-10000px";
            iframe.style.width = "0px";
            iframe.style.height = "0px";
            iframe.style.border = "0";
            iframe.style.visibility = "hidden";

            iframe.onload = function () {
                setTimeout(function () {
                    try {
                        iframe.contentWindow.focus();
                        iframe.contentWindow.print();
                    } finally {
                        setTimeout(function () {
                            URL.revokeObjectURL(url);
                            iframe.parentNode && iframe.parentNode.removeChild(iframe);
                        }, 1000);
                    }
                }, 200);
            };

            iframe.src = url;
            document.body.appendChild(iframe);
        },

        _setContractInlineError: function (sText) {
            var oInput = this.byId("step1InputContract");
            if (oInput) {
                oInput.setValueState("Error");
                oInput.setValueStateText(sText);
                oInput.focus();
            }

            var oCtx = this.getView().getBindingContext();
            var oModel = this.getView().getModel();
            if (oCtx && oModel) {
                var sTarget = oCtx.getPath() + "/SalesDocument";
                var aAll = Messaging.getMessageModel().getData() || [];
                var aOldForField = aAll.filter(function (m) {
                    return m.getTarget && m.getTarget() === sTarget;
                });
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
                var sTarget = oCtx.getPath() + "/SalesDocument";
                var aAll = Messaging.getMessageModel().getData() || [];
                var aForField = aAll.filter(function (m) {
                    return m.getTarget && m.getTarget() === sTarget;
                });
                if (aForField.length) { Messaging.removeMessages(aForField); }
            }
        },

        onCancel: function () {
            this._onObjectMatched();
        }
    });
});
