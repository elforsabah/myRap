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

                        if (!sB64) {
                            MessageBox.error("No PDF content returned from backend (missing pdfbase64).");
                            return;
                        }

                        // Ensure it's a plain Base64 string (no data: prefix; no whitespace)
                        sB64 = String(sB64);
                        sB64 = sB64.replace(/\s+/g, ""); // strip line breaks/spaces

                        // If the backend already sent a full data URL, strip the prefix to avoid doubling
                        var sPrefix = "data:application/pdf;base64,";
                        if (sB64.startsWith(sPrefix)) {
                            sB64 = sB64.slice(sPrefix.length);
                        }


                        // Build the data URL for the PDFViewer (Option A)
                        var sSrc = sPrefix + sB64;

                        // Reuse a single PDFViewer instance
                        if (!this.that._oPdfViewer) {
                            this.that._oPdfViewer = new sap.m.PDFViewer({
                                title: "PDF",
                                source: sSrc
                            });
                            this.that.getView().addDependent(this.that._oPdfViewer);
                        } else {
                            this.that._oPdfViewer.setSource(sSrc);
                        }

                        this.that._oPdfViewer.open();
                        // Optionally store last PDF in local model
                        oLocalModel.setProperty("/lastPdfBase64", sB64);

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
