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
                this.getView().getModel("local").setProperty("/contractId", "");  // Reset local value
                this.getView().getModel("local").setProperty("/mainWeight", "");  // Reset local value
                this.getView().getModel("local").setProperty("/loadType", "");  // Reset local value
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
                // Step 3: Enter on Gross/Tare inputs -> compute & advance
                var aStep3Ids = ["ipGrossWeight", "ipTareWeight"];
                aStep3Ids.forEach(function (sId) {
                    var oCtrl = this.byId(sId);
                    if (oCtrl) {
                        oCtrl.addEventDelegate({
                            onsapenter: function (oEvent) {
                                oEvent.preventDefault();
                                oEvent.stopPropagation();
                                this._onStep3Enter();
                            }.bind(this)
                        }, oCtrl);
                    }
                }.bind(this));
            },
            // Step 3 Enter handler (ADDED)
            _onStep3Enter: function () {
                var oGW = this.byId("ipGrossWeight");
                var oTW = this.byId("ipTareWeight");
                var oNW = this.byId("ipNetWeight");
                var gw = oGW ? parseFloat(oGW.getValue()) : NaN;
                var tw = oTW ? parseFloat(oTW.getValue()) : NaN;
                if (isNaN(gw) || isNaN(tw)) {
                    MessageToast.show("Enter valid numbers for Gross and Tare.");
                    return;
                }
                // persist to bound context so rebinds won't clear
                var oCtx = this.getView().getBindingContext();
                if (oCtx) {
                    oCtx.setProperty("Grossweight", gw);
                    oCtx.setProperty("TareWeight", tw);
                    oCtx.setProperty("NetWeight", gw - tw);
                }
                if (oNW) { oNW.setValue(gw - tw); }
                this.oWizard.validateStep(this.byId("step3"));
                this.oWizard.nextStep();
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
                    console.log("Waiting for entity creation...");  // Debug: Start of create chain
                    oContext.created().then(function () {
                        console.log("Entity created successfully. Invoking identifyCard...");  // Debug: Create succeeded
                        var oAction = oModel.bindContext("com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.identifyCard(...)", oContext);
                        oAction.setParameter("Vbeln", sContractId);
                        return oAction.invoke();
                    }.bind(this)).then(function (oResult) {
                        console.log("identifyCard invoked successfully.");  // Debug: Action succeeded
                        // Refresh context to pull server-set Vbeln and any other updates
                        oContext.refresh();
                        // Bind step 2 dynamically

                        var sPath = "/ZI_WR_SALESITEM_CONTRACTVH(P_SalesOrder='" + sContractId + "')/Set";  // Add /Set here
                        var oVBox = this.byId("step2LtContainer");
                        var oTemplate = new Button({
                            text: {
                                parts: [
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
                                        $select: "SalesOrder,SalesOrderitem,Material,MaterialText,Language",
                                        $orderby: "SalesOrder,SalesOrderitem"
                                    },
                                    template: oTemplate,
                                    templateShareable: false,
                                    events: {
                                        change: function (oEvent) {
                                            if (oEvent.getParameter("reason") === "Rejected") {
                                                MessageToast.show("Failed to load load types. Check contract or service.");
                                            }
                                        }
                                    }
                                });
                            } catch (oError) {
                                console.error("Binding error: ", oError);
                                this._setContractInlineError("Failed to load materials for contract.");
                                return; // Prevent advancing
                            }
                        } else {
                            console.error("step2LtContainer not found");
                        }
                        // ... (existing code to advance wizard)

                        // advance wizard
                        this._clearContractInlineError();
                        this.oWizard.validateStep(this.byId("step1"));
                        MessageToast.show("Contract is Valid. Step 2 activated")
                        this.oWizard.nextStep();
                    }.bind(this)).catch(function (oError) {
                        console.error("Error in create or identifyCard: ", oError);  // Debug: Unified catch for full error details
                        // Handle failure (original logic consolidated into one catch for simplicity)
                        var sErrorMsg = oError.message || "Unknown error";  // This captures the backend message like "Contract is Invalid"
                        this._setContractInlineError(sErrorMsg);  // Display the exact message inline
                        /*    if (oError.status === 404 || oError.status === 400) {  // Example: Customize based on common HTTP errors
                               this._setContractInlineError("Invalid Contract. Please try again.");
                           } else {
                               this._setContractInlineError("Failed to create weighing session or identify contract: " + sErrorMsg);
                           } */
                    }.bind(this));
                }
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
                this.getView().getModel("local").setProperty("/loadType", sLoadType);  // Reset local value
                //Now call the determineWeight() action from the backend and get weight that will be displayed in Step 3
                this.oWizard.validateStep(this.byId("step2"));
                this.oWizard.nextStep();
            },
            onStepActivate: function () { /* optional per-step hooks */ },
            validateStep: function () { /* return true; */ },
            onCaptureWeight: function () {
                this.getView().getModel().setProperty("/Grossweight", 1000);
            },
            onCaptureFinalWeight: function () {
                var oModel = this.getView().getModel();
                var fGross = oModel.getProperty("/Grossweight");
                var fTare = oModel.getProperty("/TareWeight");
                oModel.setProperty("/NetWeight", fGross - fTare);
            },
            onSubmit: function () {
                this.getView().getModel().submitChanges({
                    success: function () { MessageToast.show("Weighing session submitted."); }
                });
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
                    var sContractId = this.getView().getModel("local").getProperty("/contractId");
                    var sLoadType = this.getView().getModel("local").getProperty("/loadType");
                    oAction.setParameter("Vbeln", sContractId);
                    oAction.setParameter("Loadtype", sLoadType);

                    return oAction.invoke();
                }.bind(this)).then(function (oResult) {
                    console.log("determineWeight invoked successfully.");
                    // NEW: Manually fetch all messages from the message model
                    var aMessages = Messaging.getMessageModel().getData() || [];
                    // Filter for the specific success message using getCode() (expected format: 'msgid/msgno')
                    var aSpecificSuccesses = aMessages.filter(function (oMsg) {
                        return oMsg.getType() === "Success" && oMsg.getCode() === 'ZWR_WEIGHBRIGE_MESS/002';
                    });
                    if (aSpecificSuccesses.length > 0) {
                        // Take the first (or last) matching message; assuming one per action
                        var oSpecificMsg = aSpecificSuccesses[0]; // or aSpecificSuccesses[aSpecificSuccesses.length - 1]
                        var sMsgText = oSpecificMsg.getMessage();
                        oLocalModel.setProperty("/mainWeight", sMsgText);
                        MessageToast.show("Weight captured: " + sMsgText, { duration: 50000 });
                        jQuery(".sapMMessageToast").addClass("myGreenToast");

                        // Make Confirm button visible
                        // Make the CONFIRM button visible after weight is set
                        var oButton = this.byId("step3BtnConfirm");
                        if (oButton) {
                            oButton.setVisible(true);
                        }

                        // Parse the weight (assumes text like "Weight determined: 1000" or similar; adjust regex if needed)
                        /* var aMatch = sMsgText.match(/(\d+(\.\d+)?)/); // Matches integer or decimal number
                        if (aMatch) {
                            var fWeight = parseFloat(aMatch[0]);
                            oLocalModel.setProperty("/mainWeight", fWeight.toString());
                            // Optional: Show a toast with the parsed weight
                            MessageToast.show("Weight captured: " + fWeight + "KG", { duration: 50000});
                            jQuery(".sapMMessageToast").addClass("myGreenToast");
                        } else {
                            // Fallback if parsing fails
                            MessageToast.show(sMsgText); // Show original message
                        } */
                        // Remove the processed specific success messages to avoid accumulation or popups
                        Messaging.removeMessages(aSpecificSuccesses);
                    } else {
                        MessageToast.show("No specific success message (ZWR_WEIGHBRIGE_MESS/002) received from backend.");
                    }
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
                var sMainWeight = oLocalModel.getProperty("/mainWeight");

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
                          //var   sBg64 = "JVBERi0xLjcNCiW1tbW1DQoxIDAgb2JqDQo8PC9UeXBlL0NhdGFsb2cvUGFnZXMgMiAwIFIvTGFuZyhkZSkgL1N0cnVjdFRyZWVSb290IDE1IDAgUi9NYXJrSW5mbzw8L01hcmtlZCB0cnVlPj4vTWV0YWRhdGEgMjcgMCBSL1ZpZXdlclByZWZlcmVuY2VzIDI4IDAgUj4+DQplbmRvYmoNCjIgMCBvYmoNCjw8L1R5cGUvUGFnZXMvQ291bnQgMS9LaWRzWyAzIDAgUl0gPj4NCmVuZG9iag0KMyAwIG9iag0KPDwvVHlwZS9QYWdlL1BhcmVudCAyIDAgUi9SZXNvdXJjZXM8PC9Gb250PDwvRjEgNSAwIFIvRjIgMTIgMCBSPj4vRXh0R1N0YXRlPDwvR1MxMCAxMCAwIFIvR1MxMSAxMSAwIFI+Pi9Qcm9jU2V0Wy9QREYvVGV4dC9JbWFnZUIvSW1hZ2VDL0ltYWdlSV0gPj4vTWVkaWFCb3hbIDAgMCA1OTUuMzIgODQxLjkyXSAvQ29udGVudHMgNCAwIFIvR3JvdXA8PC9UeXBlL0dyb3VwL1MvVHJhbnNwYXJlbmN5L0NTL0RldmljZVJHQj4+L1RhYnMvUy9TdHJ1Y3RQYXJlbnRzIDA+Pg0KZW5kb2JqDQo0IDAgb2JqDQo8PC9GaWx0ZXIvRmxhdGVEZWNvZGUvTGVuZ3RoIDIwOT4+DQpzdHJlYW0NCniclY4/iwIxEMX7QL7DK1UwO5NNNgmEFO6uomghLlxxWAh6W91yf74/XFavUDunee8xj5kfisPXaUCMxa5eN6Biexp6TM6XedNOU8KiqfEtBSkax3vHINhgVanhDaug8XOR4m2GQYpFJ0WxZLBG9yHFWCUwHCmvDZwNyht0n7m0OjCh/82H0d8i/8eVFO+RKJTJhaxLk1wktnWqslSWyiaMlrwh0jrNR1+2ycTr5ohuI0WbOfZSvMitn7mZnfLVPfeV9sY4wfThGdpdjT95qULTDQplbmRzdHJlYW0NCmVuZG9iag0KNSAwIG9iag0KPDwvVHlwZS9Gb250L1N1YnR5cGUvVHlwZTAvQmFzZUZvbnQvQkNERUVFK0FwdG9zL0VuY29kaW5nL0lkZW50aXR5LUgvRGVzY2VuZGFudEZvbnRzIDYgMCBSL1RvVW5pY29kZSAyMyAwIFI+Pg0KZW5kb2JqDQo2IDAgb2JqDQpbIDcgMCBSXSANCmVuZG9iag0KNyAwIG9iag0KPDwvQmFzZUZvbnQvQkNERUVFK0FwdG9zL1N1YnR5cGUvQ0lERm9udFR5cGUyL1R5cGUvRm9udC9DSURUb0dJRE1hcC9JZGVudGl0eS9EVyAxMDAwL0NJRFN5c3RlbUluZm8gOCAwIFIvRm9udERlc2NyaXB0b3IgOSAwIFIvVyAyNSAwIFI+Pg0KZW5kb2JqDQo4IDAgb2JqDQo8PC9PcmRlcmluZyhJZGVudGl0eSkgL1JlZ2lzdHJ5KEFkb2JlKSAvU3VwcGxlbWVudCAwPj4NCmVuZG9iag0KOSAwIG9iag0KPDwvVHlwZS9Gb250RGVzY3JpcHRvci9Gb250TmFtZS9CQ0RFRUUrQXB0b3MvRmxhZ3MgMzIvSXRhbGljQW5nbGUgMC9Bc2NlbnQgOTM5L0Rlc2NlbnQgLTI4Mi9DYXBIZWlnaHQgOTM5L0F2Z1dpZHRoIDU2MS9NYXhXaWR0aCAxNjgyL0ZvbnRXZWlnaHQgNDAwL1hIZWlnaHQgMjUwL1N0ZW1WIDU2L0ZvbnRCQm94WyAtNTAwIC0yODIgMTE4MiA5MzldIC9Gb250RmlsZTIgMjQgMCBSPj4NCmVuZG9iag0KMTAgMCBvYmoNCjw8L1R5cGUvRXh0R1N0YXRlL0JNL05vcm1hbC9jYSAxPj4NCmVuZG9iag0KMTEgMCBvYmoNCjw8L1R5cGUvRXh0R1N0YXRlL0JNL05vcm1hbC9DQSAxPj4NCmVuZG9iag0KMTIgMCBvYmoNCjw8L1R5cGUvRm9udC9TdWJ0eXBlL1RydWVUeXBlL05hbWUvRjIvQmFzZUZvbnQvQkNERkVFK0FwdG9zL0VuY29kaW5nL1dpbkFuc2lFbmNvZGluZy9Gb250RGVzY3JpcHRvciAxMyAwIFIvRmlyc3RDaGFyIDMyL0xhc3RDaGFyIDMyL1dpZHRocyAyNiAwIFI+Pg0KZW5kb2JqDQoxMyAwIG9iag0KPDwvVHlwZS9Gb250RGVzY3JpcHRvci9Gb250TmFtZS9CQ0RGRUUrQXB0b3MvRmxhZ3MgMzIvSXRhbGljQW5nbGUgMC9Bc2NlbnQgOTM5L0Rlc2NlbnQgLTI4Mi9DYXBIZWlnaHQgOTM5L0F2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TTBNcENlaGlIenJlU3pOVGN6a2M5ZCI/Pjx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IjMuMS03MDEiPgo8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPgo8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiAgeG1sbnM6cGRmPSJodHRwOi8vbnMuYWRvYmUuY29tL3BkZi8xLjMvIj4KPHBkZjpQcm9kdWNlcj5NaWNyb3NvZnTCriBXb3JkIGbDvHIgTWljcm9zb2Z0IDM2NTwvcGRmOlByb2R1Y2VyPjwvcmRmOkRlc2NyaXB0aW9uPgo8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiAgeG1sbnM6ZGM9Imh0dHA6Ly9wdXJsLm9yZy9kYy9lbGVtZW50cy8xLjEvIj4KPGRjOmNyZWF0b3I+PHJkZjpTZXE+PHJkZjpsaT5FbHZpcyBGb3JzYWI8L3JkZjpsaT48L3JkZjpTZXE+PC9kYzpjcmVhdG9yPjwvcmRmOkRlc2NyaXB0aW9uPgo8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiAgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIj4KPHhtcDpDcmVhdG9yVG9vbD5NaWNyb3NvZnTCriBXb3JkIGbDvHIgTWljcm9zb2Z0IDM2NTwveG1wOkNyZWF0b3JUb29sPjx4bXA6Q3JlYXRlRGF0ZT4yMDI1LTA5LTA3VDA5OjA4OjQ4KzAyOjAwPC94bXA6Q3JlYXRlRGF0ZT48eG1wOk1vZGlmeURhdGU+MjAyNS0wOS0wN1QwOTowODo0OCswMjowMDwveG1wOk1vZGlmeURhdGU+PC9yZGY6RGVzY3JpcHRpb24+CjxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSIiICB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyI+Cjx4bXBNTTpEb2N1bWVudElEPnV1aWQ6NEJBNUEyQkItNjk5RC00MjQ3LUE3NkEtNDUzRDNCQjA0ODQzPC94bXBNTTpEb2N1bWVudElEPjx4bXBNTTpJbnN0YW5jZUlEPnV1aWQ6NEJBNUEyQkItNjk5RC00MjQ3LUE3NkEtNDUzRDNCQjA0ODQzPC94bXBNTTpJbnN0YW5jZUlEPjwvcmRmOkRlc2NyaXB0aW9uPgogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIAogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAKPC9yZGY6UkRGPjwveDp4bXBtZXRhPjw/eHBhY2tldCBlbmQ9InciPz4NCmVuZHN0cmVhbQ0KZW5kb2JqDQoyOCAwIG9iag0KPDwvRGlzcGxheURvY1RpdGxlIHRydWU+Pg0KZW5kb2JqDQoyOSAwIG9iag0KPDwvVHlwZS9YUmVmL1NpemUgMjkvV1sgMSA0IDJdIC9Sb290IDEgMCBSL0luZm8gMTQgMCBSL0lEWzxCQkEyQTU0QjlENjk0NzQyQTc2QTQ1M0QzQkIwNDg0Mz48QkJBMkE1NEI5RDY5NDc0MkE3NkE0NTNEM0JCMDQ4NDM+XSAvRmlsdGVyL0ZsYXRlRGVjb2RlL0xlbmd0aCAxMDc+Pg0Kc3RyZWFtDQp4nC3MvRFAQBAF4Hd/GCICPWhAQipQFTUYowG5TKQPiRIUIDxnnw32m9ndt0Ao71XoOfCxkktQj2AKMpFNsD3h0A1kJ7cQcRePgA4/S2hiiCWOKPJfRiGXLBJPWyGbhaojBzmFugFesK0PKA0KZW5kc3RyZWFtDQplbmRvYmoNCnhyZWYNCjAgMzANCjAwMDAwMDAwMTUgNjU1MzUgZg0KMDAwMDAwMDAxNyAwMDAwMCBuDQowMDAwMDAwMTYzIDAwMDAwIG4NCjAwMDAwMDAyMTkgMDAwMDAgbg0KMDAwMDAwMDUwMyAwMDAwMCBuDQowMDAwMDAwNzg2IDAwMDAwIG4NCjAwMDAwMDA5MTQgMDAwMDAgbg0KMDAwMDAwMDk0MiAwMDAwMCBuDQowMDAwMDAxMDk3IDAwMDAwIG4NCjAwMDAwMDExNzAgMDAwMDAgbg0KMDAwMDAwMTQwNyAwMDAwMCBuDQowMDAwMDAxNDYxIDAwMDAwIG4NCjAwMDAwMDE1MTUgMDAwMDAgbg0KMDAwMDAwMTY4MiAwMDAwMCBuDQowMDAwMDAxOTIwIDAwMDAwIG4NCjAwMDAwMDAwMTYgNjU1MzUgZg0KMDAwMDAwMDAxNyA2NTUzNSBmDQowMDAwMDAwMDE4IDY1NTM1IGYNCjAwMDAwMDAwMTkgNjU1MzUgZg0KMDAwMDAwMDAyMCA2NTUzNSBmDQowMDAwMDAwMDIxIDY1NTM1IGYNCjAwMDAwMDAwMjIgNjU1MzUgZg0KMDAwMDAwMDAwMCA2NTUzNSBmDQowMDAwMDAyNjIwIDAwMDAwIG4NCjAwMDAwMDI5NjQgMDAwMDAgbg0KMDAwMDAxMDU2NSAwMDAwMCBuDQowMDAwMDEwNjgzIDAwMDAwIG4NCjAwMDAwMTA3MTAgMDAwMDAgbg0KMDAwMDAxMzg4MyAwMDAwMCBuDQowMDAwMDEzOTI4IDAwMDAwIG4NCnRyYWlsZXINCjw8L1NpemUgMzAvUm9vdCAxIDAgUi9JbmZvIDE0IDAgUi9JRFs8QkJBMkE1NEI5RDY5NDc0MkE3NkE0NTNEM0JCMDQ4NDM+PEJCQTJBNTRCOUQ2OTQ3NDJBNzZBNDUzRDNCQjA0ODQzPl0gPj4NCnN0YXJ0eHJlZg0KMTQyMzYNCiUlRU9GDQp4cmVmDQowIDANCnRyYWlsZXINCjw8L1NpemUgMzAvUm9vdCAxIDAgUi9JbmZvIDE0IDAgUi9JRFs8QkJBMkE1NEI5RDY5NDc0MkE3NkE0NTNEM0JCMDQ4NDM+PEJCQTJBNTRCOUQ2OTQ3NDJBNzZBNDUzRDNCQjA0ODQzPl0gL1ByZXYgMTQyMzYvWFJlZlN0bSAxMzkyOD4+DQpzdGFydHhyZWYNCjE0OTkzDQolJUVPRg==";
                          
                     
                           if (sB64 && typeof sB64 === "string") {
                            // Build a data URL and convert to Blob (safer for large PDFs and printing)
                            var dataUrl = "data:application/pdf;base64," + sB64;

                            fetch(dataUrl)
                                .then(function (r) { return r.blob(); })
                                .then(function (blob) {
                                    var url = URL.createObjectURL(blob);

                                    // Try to open in a new tab with an iframe (good for print)
                                    var win = window.open("");
                                    if (win && win.document) {
                                        win.document.title = "weighing_slip.pdf";
                                        win.document.body.style.margin = "0";
                                        var iframe = win.document.createElement("iframe");
                                        iframe.style.border = "0";
                                        iframe.style.width = "100%";
                                        iframe.style.height = "100%";
                                        iframe.src = url;
                                        win.document.body.appendChild(iframe);

                                        // Optional: auto-print once loaded
                                        // iframe.onload = function () {
                                        //     try { iframe.contentWindow && iframe.contentWindow.print(); } catch (e) {}
                                        // };
                                    } else {
                                        // Popup blocked → download fallback
                                        var link = document.createElement("a");
                                        link.href = url;
                                        link.download = "weighing_slip.pdf";
                                        document.body.appendChild(link);
                                        link.click();
                                        document.body.removeChild(link);
                                    }

                                    MessageToast.show("PDF generated.");
                                })
                                .catch(function () {
                                    // Last-resort: open the data URL directly
                                    var w = window.open(dataUrl, "_blank");
                                    if (!w) {
                                        var link = document.createElement("a");
                                        link.href = dataUrl;
                                        link.download = "weighing_slip.pdf";
                                        link.click();
                                    }
                                });
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

                        // Refresh & proceed in the wizard
                        oContext.refresh();
                        this.oWizard.nextStep();
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


            // Helper method to print PDF from base64
            _printPdfFromBase64: function (sBase64) {
                if (!sBase64) {
                    MessageToast.show("No PDF data available to print.");
                    return;
                }
                // Create a data URL for the PDF
                var sPdfDataUrl = "data:application/pdf;base64," + sBase64;
                // Open in a new window and trigger print
                var oPrintWindow = window.open("", "_blank");
                if (oPrintWindow) {
                    oPrintWindow.document.write('<iframe src="' + sPdfDataUrl + '" frameborder="0" style="border:0; top:0px; left:0px; bottom:0px; right:0px; width:100%; height:100%;" allowfullscreen></iframe>');
                    oPrintWindow.document.close();
                    // Wait for iframe to load before printing (optional timeout)
                    setTimeout(function () {
                        oPrintWindow.focus();
                        oPrintWindow.print();
                    }, 1000); // Adjust timeout if needed
                } else {
                    MessageToast.show("Failed to open print window. Please allow pop-ups.");
                }
            },


            onPrintSlip: function () { },
            onWizardComplete: function () { }
        });
    });



