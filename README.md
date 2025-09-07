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

                        if (typeof sB64 === "string") {
                            sB64 = String(sB64);
                        }

                        
                        if (sB64 && typeof sB64 === "string") {
                            try {
                                var blob = this._base64ToBlob(sB64, "application/pdf");
                                var url = URL.createObjectURL(blob);

                                // Open in new tab with iframe (rest of your code remains the same)
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

                                    // Optional: Auto-print
                                    // iframe.onload = function () {
                                    //     try { iframe.contentWindow.print(); } catch (e) {}
                                    // };
                                } else {
                                    // Fallback: Download
                                    var link = document.createElement("a");
                                    link.href = url;
                                    link.download = "weighing_slip.pdf";
                                    document.body.appendChild(link);
                                    link.click();
                                    document.body.removeChild(link);
                                }

                                // Revoke the URL after use to free memory
                                setTimeout(function () {
                                    URL.revokeObjectURL(url);
                                }, 60000); // Adjust timeout as needed

                                MessageToast.show("PDF generated.");
                            } catch (oError) {
                                console.error("Base64 to Blob conversion failed: ", oError);
                                MessageToast.show("Failed to process PDF data.");
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
