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
        oAction.invoke().then(function () {
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
