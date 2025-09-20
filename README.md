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
    var sGrossWeight = oLocalModel.getProperty("/grossWeight");
    var sTeraWeight = oLocalModel.getProperty("/teraWeight");
    if (!sContractId || !sLoadType || !sGrossWeight) {
        MessageToast.show("Missing required data: Contract ID, Load Type, or Weight.");
        return;
    }
    // Parse "12345 KG" (fallback unit KG)
    var aWeightParts = sGrossWeight.trim().split(/\s+/);
    var sWeight = aWeightParts[1] || "";
    var sWeighUnit = aWeightParts[2] || "KG";
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
        // Now call the silent print function
        if (sTeraWeight) {                     // Print only when there is Tera Weight
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
                        
                        this._printBase64PdfSilently(sB64);  // Print only when there is Teraweight
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
                console.log("All Messages 3", aMsgs)
                var aSucc = aMsgs.filter(function (m) { return m.getType && m.getType() === "Success"; });
                if (aSucc.length > 0) {
                    MessageToast.show(aSucc[0].getMessage());
                    Messaging.removeMessages(aSucc);
                }
                // Blur any focused element to prevent ARIA warning when hiding the step
                if (document.activeElement) {
                    document.activeElement.blur();
                }
                // Refresh & reset to step 1 instead of next step
                oContext.refresh();
                this._onObjectMatched();
            }.bind(this));
        } else {
            // Blur any focused element to prevent ARIA warning when hiding the step
            if (document.activeElement) {
                document.activeElement.blur();
            }
           // Refresh & reset to step 1 instead of next step
            oContext.refresh();
            this._onObjectMatched();
        }

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
