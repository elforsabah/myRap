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
    var aWeightParts = String(sMainWeight).trim().split(/\s+/);
    var sWeight = aWeightParts[0] || "";
    var sWeighUnit = aWeightParts[1] || "KG";

    // Pad contract like in step 1
    sContractId = String(sContractId).padStart(10, "0");

    // === Helpers: normalize Base64 and convert to Blob (PDF) ===
    function normalizeToStdBase64(val) {
        // If backend already gave binary (Uint8Array/ArrayBuffer), return as-is
        if (val instanceof Uint8Array) return { kind: "u8", data: val };
        if (val && val.buffer instanceof ArrayBuffer) return { kind: "u8", data: new Uint8Array(val) };

        var s = String(val || "").trim();
        // strip any data URL prefix
        s = s.replace(/^data:.*;base64,/, "");
        // remove whitespace/newlines
        s = s.replace(/\s+/g, "");
        // convert base64url to base64
        s = s.replace(/-/g, "+").replace(/_/g, "/");
        // pad to multiple of 4
        var pad = s.length % 4;
        if (pad) s += "=".repeat(4 - pad);

        return { kind: "b64", data: s };
    }

    function toPdfBlob(maybeB64OrU8) {
        var normalized = normalizeToStdBase64(maybeB64OrU8);

        if (normalized.kind === "u8") {
            return new Blob([normalized.data], { type: "application/pdf" });
        }
        var sB64 = normalized.data;
        var byteString = atob(sB64); // now safe
        var len = byteString.length;
        var bytes = new Uint8Array(len);
        for (var i = 0; i < len; i++) bytes[i] = byteString.charCodeAt(i);
        return new Blob([bytes], { type: "application/pdf" });
    }
    // === end helpers ===

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

            // Support both names/casings. For xstring/Edm.Binary, value is Base64 on the wire.
            var sB64 = oRes && (
                oRes.pdfraw || oRes.Pdfraw || oRes.PDFRAW ||
                oRes.pdfbase64 || oRes.Pdfbase64 || oRes.PDFBASE64
            );

            if (!sB64) {
                MessageBox.error("No PDF content returned from backend (missing pdfbase64).");
                return;
            }

            // ==== Option B: normalize -> Blob -> ObjectURL ====
            var oBlob = toPdfBlob(sB64);
            var sUrl  = URL.createObjectURL(oBlob);

            // a) Open in a new tab
            window.open(sUrl);

            // b) Or force download (uncomment to use download instead)
            /*
            var a = document.createElement("a");
            a.href = sUrl;
            a.download = "document.pdf";
            a.click();
            */

            // Cleanup
            setTimeout(function () {
                URL.revokeObjectURL(sUrl);
            }, 60000);

            // Optionally store last (normalized) Base64 in local model
            // (For traceability; if you prefer, store the Blob URL instead)
            oLocalModel.setProperty("/lastPdfBase64", String(sB64));

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
