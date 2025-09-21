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
                        // Get the returned context from action
                        var oContext = oAction.getBoundContext();
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
                                this._setContractInlineError(this.getResourceBundle().getText("failedMaterials"));
                                return; // Prevent advancing
                            }
                        } else {
                            console.error("step2LtContainer not found");
                        }

                        //Manually fetch all messages from the message model
                        var aMessages = Messaging.getMessageModel().getData() || [];
                        // Filter for the specific success message using getCode() (expected format: 'msgid/msgno')
                        var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                            return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/007';
                        });

                        if (aSpecificSuccesses.length > 0) {
                            this.getView().getModel("local").setProperty("/transaction_state", "2. Weighing"); // Reset local value
                        }
                        else {
                            this.getView().getModel("local").setProperty("/transaction_state", "1. Weighing"); // Reset local value
                        }
                        // advance wizard
                        this._clearContractInlineError();
                        this.oWizard.validateStep(this.byId("step1"));
                        MessageToast.show(this.getResourceBundle().getText("contractValid"))
                        this.oWizard.nextStep();
                    }.bind(this)).catch(function (oError) {
                        console.error("Error in identifyCard: ", oError); // Debug: Unified catch for full error details
                        // Handle failure (original logic consolidated into one catch for simplicity)
                        var sErrorMsg = oError.message || "Unknown error"; // This captures the backend message like "Contract is Invalid"
                        this._setContractInlineError(sErrorMsg); // Display the exact message inline
                    }.bind(this));
                }
            },
            _skipToWeigh: function () {
                var oCtx = this.getView().getBindingContext();
                if (oCtx) {
                    this.getView().getModel("local").setProperty("/loadType", "");
                }
                this.oWizard.validateStep(this.byId("step2"));
                this.onWeighStep3();
                this.oWizard.nextStep();
            },
            onAfterRendering: function () {
                var oIp = this.byId("step1InputContract");
                if (oIp) { oIp.focus(); }
                this._setupEnterToNext(); // wire Enter once
            },
            onChooseLoadType: function (oEvent) {
                const sLoadType = oEvent.getSource().getBindingContext().getProperty("Material");
                const oSessionCtx = this.getView().getBindingContext();
                if (!oSessionCtx) { return; }
                this.getView().getModel("local").setProperty("/loadType", sLoadType);
                //Now call the determineWeight() action from the backend and get weight that will be displayed in Step 3
                this.onWeighStep3();
                this.oWizard.validateStep(this.byId("step2"));
                this.oWizard.nextStep();
            },
            onStepActivate: function (oEvent) {
                var oStep = oEvent.getParameter("step");
                if (!oStep) { return; } // Guard against undefined step
                var sId = oStep.getId().split("--").pop();
                // Removed call to onWeighStep3 here to avoid double invocation
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
                oAction.invoke().then(function (oResult) {
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
                        // Remove the processed specific success messages to avoid accumulation or popups
                        // Make Confirm button visible
                        // Make the CONFIRM button visible after weight is set
                        var oButton = this.byId("step3BtnConfirm");
                        if (oButton) {
                            oButton.setVisible(true);
                        }
                        var oButton = this.byId("step3BtnConfirm2");
                        if (oButton) {
                            oButton.setVisible(false);
                        }
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
                if (sTeraWeight) {                     // Print only when there is Tera Weight
                    oAction.invoke().then(function () {
                        // Get the returned context from action
                        var oContext = oAction.getBoundContext();
                        this.getView().setBindingContext(oContext);
                        // Read the result payload from the bound context
                        var oResCtx = oAction.getBoundContext();
                        var oRes = oResCtx && oResCtx.getObject ? oResCtx.getObject() : null;
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

                                this._printBase64PdfSilently(sB64);  // Print only when there is Teraweight
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
                        console.log("All Messages 3", aMsgs)
                        var aSucc = aMsgs.filter(function (m) { return m.getType && m.getType() === "Success"; });
                        if (aSucc.length > 0) {
                            MessageToast.show(aSucc[0].getMessage());
                            Messaging.removeMessages(aSucc);
                        }
                        // Refresh & reset to step 1 instead of next step
                        oContext.refresh();
                        this._onObjectMatched();
                    }.bind(this)).catch(function (oError) {
                        var sErr = (oError && oError.message) || this.getResourceBundle().getText("failedPdf");
                        MessageToast.show(sErr);
                        // Optional: surface server messages (Error)
                        var aMsgs = Messaging.getMessageModel().getData() || [];
                        var aErrs = aMsgs.filter(function (m) { return m.getType && m.getType() === "Error"; });
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
            }
        });
    });
