onConfirmStep3: function () {
    var oContext     = this.getView().getBindingContext();
    var oModel       = this.getView().getModel();
    var oLocalModel  = this.getView().getModel("local");

    if (!oContext) {
        sap.m.MessageToast.show("No session context available.");
        return;
    }

    var sContractId = oLocalModel.getProperty("/contractId");
    var sLoadType   = oLocalModel.getProperty("/loadType");
    var sMainWeight = oLocalModel.getProperty("/mainWeight");

    if (!sContractId || !sLoadType || !sMainWeight) {
        sap.m.MessageToast.show("Missing required data: Contract ID, Load Type, or Weight.");
        return;
    }

    // Parse "12345 KG" (fallback unit KG)
    var aWeightParts = String(sMainWeight).trim().split(/\s+/);
    var sWeight      = aWeightParts[0] || "";
    var sWeighUnit   = aWeightParts[1] || "KG";

    // Pad contract like in step 1
    sContractId = String(sContractId).padStart(10, "0");

    // --- call your backend action as you already do ---
    // IMPORTANT: Keep your existing logic here. The only thing we need
    // is the result object 'oRes' that contains the Base64 PDF.
    // Example using EditFlow (adjust action name/parameters as needed):
    var that = this;
    this.editFlow.invokeAction(
        "com.sap.gateway.srvd.zsb_wr_weighingbrige.v0001.printSlip", // <-- use your real action name
        {
            contexts: [oContext],
            // If your action expects parameters, pass them here:
            // parameterValues: { ContractId: sContractId, LoadType: sLoadType, Weight: sWeight, Unit: sWeighUnit }
        }
    ).then(function (oResponse) {

        // oResponse can be a context or an array; get the payload object safely
        var oRes;
        try {
            if (oResponse && typeof oResponse.getObject === "function") {
                oRes = oResponse.getObject(); // V4 single context
            } else if (Array.isArray(oResponse) && oResponse[0] && typeof oResponse[0].getObject === "function") {
                oRes = oResponse[0].getObject(); // V4 multiple contexts
            } else if (oResponse && oResponse.result) {
                oRes = oResponse.result; // fallback shape
            } else {
                oRes = oResponse; // last resort
            }
        } catch (e) {
            oRes = oResponse;
        }

        // --- Extract Base64 robustly (handles various property casings) ---
        var sB64 = oRes && (
            oRes.pdfbase64  || oRes.Pdfbase64  || oRes.PDFBASE64 ||
            oRes.pdfraw     || oRes.Pdfraw     || oRes.PDFRAW    ||
            oRes.base64     || oRes.Base64
        );

        if (!sB64) {
            sap.m.MessageBox.error("No PDF content returned from backend (missing pdfbase64).");
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

        // ----- Option B: Base64 -> Blob -> ObjectURL -----
        function b64ToBlobPdf(b64) {
            var byteString = atob(b64);
            var len = byteString.length;
            var bytes = new Uint8Array(len);
            for (var i = 0; i < len; i++) {
                bytes[i] = byteString.charCodeAt(i);
            }
            return new Blob([bytes], { type: "application/pdf" });
        }

        var oBlob = b64ToBlobPdf(sB64);
        var sUrl  = URL.createObjectURL(oBlob);

        // a) Open in new tab
        window.open(sUrl);

        // b) Or force download (uncomment to use download instead of new tab)
        /*
        var a = document.createElement("a");
        a.href = sUrl;
        a.download = "document.pdf";
        a.click();
        */

        // Cleanup after a while
        setTimeout(function () {
            URL.revokeObjectURL(sUrl);
        }, 60000);

        // Optionally store last PDF in local model
        oLocalModel.setProperty("/lastPdfBase64", sB64);

    }).catch(function (err) {
        // Centralized error handling
        var sMsg = (err && err.message) || String(err) || "Unknown error while generating PDF.";
        sap.m.MessageBox.error(sMsg);
        // console.error(err); // keep if you want logs
    });
}
