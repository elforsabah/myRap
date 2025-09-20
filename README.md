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
                this.getView().setModel(new JSONModel({ transaction_state: "" }), "local");
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
                oSessionCtx.setProperty("LoadType", sLoadType);
                this.getView().getModel("local").setProperty("/loadType", sLoadType); // Reset local value
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
                    MessageToast.show("No session context available.");
                    return;
                }
                // Ensure the entity is persisted before calling the action
                oModel.submitBatch("weighingGroup").then(function () {
                    console.log("Entity persisted. Invoking determineWeight...");
                    // Call the determineWeight action
                    var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.determineWeight(...)", oContext);
                    var sContractId = this.getView().getModel("local").getProperty("/contractId").padStart(10, '0');
                    var sLoadType = this.getView().getModel("local").getProperty("/loadType");
                    oAction.setParameter("Vbeln", sContractId);
                    oAction.setParameter("Loadtype", sLoadType);
                    return oAction.invoke();
                }.bind(this)).then(function (oResult) {
                    console.log("determineWeight invoked successfully.");
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
                        MessageToast.show("No specific success message (ZWR_WEIGHBRIGE_MESS/002) received from backend.");
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

                    MessageToast.show("Weight captured: " + sGrossweight1 + "\n" + steraweight1, { duration: 5000 });
                    jQuery(".sapMMessageToast").addClass("myGreenToast");

                    // Refresh context for any other updates
                    oContext.refresh();
                }.bind(this)).catch(function (oError) {
                    console.error("Error in determineWeight: ", oError);
                    var sErrorMsg = oError.message || "Failed to determine weight.";
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
                    MessageToast.show("No session context available.");
                    return;
                }
                var sContractId = oLocalModel.getProperty("/contractId");
                var sLoadType = oLocalModel.getProperty("/loadType");
                var sMainWeight = oLocalModel.getProperty("/grossWeight");
                if (!sContractId || !sLoadType || !sMainWeight) {
                    MessageToast.show("Missing required data: Contract ID, Load Type, or Weight.");
                    return;
                }
                // Parse "12345 KG" (fallback unit KG)
                var aWeightParts = sMainWeight.trim().split(/\s+/);
                var sWeight = aWeightParts[0] || "";
                var sWeighUnit = aWeightParts[1] || "KG";
                // Pad contract like in step 1
                sContractId = sContractId.padStart(10, "0");
                // Ensure current draft changes are sent first
                oModel.submitBatch("weighingGroup").then(function () {
                    // Bind the action to the same context
                    var oAction = oModel.bindContext(
                        "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.printSlip(...)",
                        oContext
                    );
                    oAction.setParameter("Vbeln", sContractId);
                    oAction.setParameter("Loadtype", sLoadType);
                    oAction.setParameter("Weight", sWeight);
                    oAction.setParameter("WeighUnit", sWeighUnit);
                    return oAction.invoke().then(function () {
                        // Read the result payload from the bound context
                        var oResCtx = oAction.getBoundContext();
                        var oRes = oResCtx && oResCtx.getObject ? oResCtx.getObject() : null;
                        // Support both names/casings. For xstring/Edm.Binary, property value is Base64 on the wire.
                        var sB64 = oRes && (oRes.pdfraw || oRes.Pdfraw || oRes.PDFRAW ||
                            oRes.pdfbase64 || oRes.Pdfbase64 || oRes.PDFBASE64);
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
                                // Now call the silent print function
                                var steraweight12 = oLocalModel.getProperty("/teraWeight");
                                if (steraweight12) { 
                                this._printBase64PdfSilently(sB64);  // Print only when there is Teraweight
                                }
                                MessageToast.show("PDF sent to printer.");
                            } catch (oError) {
                                console.error("Base64 processing or printing failed: ", oError);
                                MessageToast.show("Failed to process or print PDF.");
                            }
                        } else {
                            MessageToast.show("No PDF returned by printSlip.");
                        }
                        // Optional: surface server messages (Success)
                        var aMsgs = Messaging.getMessageModel().getData() || [];
                        var aSucc = aMsgs.filter(function (m) { return m.getType && m.getType() === "Success"; });
                        if (aSucc.length > 0) {
                            MessageToast.show(aSucc[0].getMessage());
                            Messaging.removeMessages(aSucc);
                        }
                        // Refresh & reset to step 1 instead of next step
                        oContext.refresh();
                        this._onObjectMatched();
                    }.bind(this));
                }.bind(this)).catch(function (oError) {
                    var sErr = (oError && oError.message) || "Failed to process print slip.";
                    MessageToast.show(sErr);
                    // Optional: surface server messages (Error)
                    var aMsgs = Messaging.getMessageModel().getData() || [];
                    var aErrs = aMsgs.filter(function (m) { return m.getType && m.getType() === "Error"; });
                    if (aErrs.length > 0) {
                        MessageToast.show(aErrs[0].getMessage());
                        Messaging.removeMessages(aErrs);
                    }
                }.bind(this));
            },
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            //// Printing in Kiosk Mode for Chrome
            /////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
            // Controller method
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
                iframe.style.position = "fixed";
                iframe.style.right = "0";
                iframe.style.bottom = "0";
                iframe.style.width = "0";
                iframe.style.height = "0";
                iframe.style.border = "0";
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
                    var sTarget = oCtx.getPath() + "/Vbeln"; // property bound to the input
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
                    var sTarget = oCtx.getPath() + "/Vbeln";
                    var aAll = Messaging.getMessageModel().getData() || [];
                    var aForField = aAll.filter(function (m) { return m.getTarget && m.getTarget() === sTarget; });
                    if (aForField.length) { Messaging.removeMessages(aForField); }
                }
            }
        });
    });
