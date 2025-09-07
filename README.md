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

            // Base64 field (various casings supported)
            var sB64 = oRes && (oRes.pdfraw || oRes.Pdfraw || oRes.PDFRAW ||
                                oRes.pdfbase64 || oRes.Pdfbase64 || oRes.PDFBASE64);

            if (typeof sB64 === "string") {
                sB64 = String(sB64);
            }

            if (sB64 && typeof sB64 === "string") {
                try {
                    // Convert Base64 → Blob (uses your robust helper)
                    var blob = this._base64ToBlob(sB64, "application/pdf");
                    var url  = URL.createObjectURL(blob);

                    // --- Direct print via hidden iframe (no new window) ---
                    var iframe = document.createElement("iframe");
                    iframe.style.position = "fixed";
                    iframe.style.right = "0";
                    iframe.style.bottom = "0";
                    iframe.style.width = "0";
                    iframe.style.height = "0";
                    iframe.style.border = "0";
                    iframe.src = url;
                    document.body.appendChild(iframe);

                    var cleanup = function () {
                        setTimeout(function () {
                            try { URL.revokeObjectURL(url); } catch (e) {}
                            if (iframe && iframe.parentNode) {
                                iframe.parentNode.removeChild(iframe);
                            }
                        }, 1000);
                    };

                    // When the PDF is loaded, trigger print
                    iframe.onload = function () {
                        try {
                            // Small delay improves reliability across browsers
                            setTimeout(function () {
                                var w = iframe.contentWindow || iframe;
                                if (w) {
                                    try { w.focus(); } catch (e) {}
                                    try { w.print(); } catch (e) {}
                                }
                                cleanup();
                            }, 150);
                        } catch (e) {
                            cleanup();
                            // Fallback: download if printing failed
                            var a = document.createElement("a");
                            a.href = url;
                            a.download = "weighing_slip.pdf";
                            document.body.appendChild(a);
                            a.click();
                            document.body.removeChild(a);
                        }
                    };

                    // Safety fallback if onload never fires
                    setTimeout(function () {
                        if (!iframe.contentDocument) {
                            // last resort: try opening to let user print manually
                            window.open(url);
                            cleanup();
                        }
                    }, 5000);

                    MessageToast.show("Sending PDF to printer…");
                } catch (oError) {
                    /* eslint-disable no-console */
                    console.error("Base64 to Blob/print failed: ", oError);
                    /* eslint-enable no-console */
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
