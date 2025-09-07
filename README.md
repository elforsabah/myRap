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

            _base64FromBinary: function (binaryData) {
                if (typeof binaryData === "string") {
                    // If it's already a base64url string (OData V4 default), convert to standard base64
                    var base64 = binaryData.replace(/-/g, '+').replace(/_/g, '/');
                    while (base64.length % 4) {
                        base64 += '=';
                    }
                    return base64;
                }

                if (binaryData instanceof ArrayBuffer || binaryData instanceof Uint8Array) {
                    var binaryString = '';
                    var bytes = new Uint8Array(binaryData instanceof ArrayBuffer ? binaryData : binaryData.buffer);
                    for (var i = 0; i < bytes.byteLength; i++) {
                        binaryString += String.fromCharCode(bytes[i]);
                    }
                    return window.btoa(binaryString);
                }

                if (binaryData instanceof Blob) {
                    return new Promise(function (resolve, reject) {
                        var reader = new FileReader();
                        reader.onload = function () {
                            resolve(reader.result.split(",")[1]);
                        };
                        reader.onerror = reject;
                        reader.readAsDataURL(binaryData);
                    });
                }

                console.error("Unsupported type for binaryData:", typeof binaryData);
                return null;
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

                        // Support both names/casings. For xstring/Edm.Binary, property value might be binary or base64 string.
                        var binaryPdf = oRes && (oRes.pdfraw || oRes.Pdfraw || oRes.PDFRAW ||
                            oRes.pdfbase64 || oRes.Pdfbase64 || oRes.PDFBASE64);

                        if (binaryPdf) {
                            var sB64Promise = this._base64FromBinary(binaryPdf);

                            if (sB64Promise instanceof Promise) {
                                // For Blob case
                                sB64Promise.then(function (sB64) {
                                    this._handlePdfBase64(sB64); // Define a helper to handle the dataUrl logic
                                }.bind(this)).catch(function (err) {
                                    console.error("Error converting Blob to base64:", err);
                                    MessageToast.show("Failed to process PDF.");
                                });
                            } else {
                                this._handlePdfBase64(sB64Promise);
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

            _handlePdfBase64: function (sB64) {
                if (typeof sB64 !== "string" || !sB64) {
                    MessageToast.show("Invalid PDF data.");
                    return;
                }

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
                    .catch(function (err) {
                        console.error("Error fetching PDF blob:", err);
                        // Last-resort: open the data URL directly
                        var w = window.open(dataUrl, "_blank");
                        if (!w) {
                            var link = document.createElement("a");
                            link.href = dataUrl;
                            link.download = "weighing_slip.pdf";
                            link.click();
                        }
                    });
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
