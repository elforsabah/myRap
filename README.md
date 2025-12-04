onNextFromK: function () {
    var oView       = this.getView();
    var oLocalModel = oView.getModel("local");
    var oModel      = oView.getModel();
    var sKorselsnr  = oLocalModel.getProperty("/korselsnr");

    // (Optional) Required check + padding
    if (!sKorselsnr) {
        MessageToast.show("Please enter Korselsnr");
        return;
    }
    // If you need 70 chars:
    // sKorselsnr = sKorselsnr.padStart(70, "0");
    // oLocalModel.setProperty("/korselsnr", sKorselsnr);

    var oSessionCtx = oView.getBindingContext();
    if (!oSessionCtx) {
        MessageToast.show("No session context available");
        return;
    }

    // --- 1) Get current path (might include identifyCard(...)) ---
    var sBasePath = oSessionCtx.getPath(); // e.g. "/ZI_WR_R_WEIGHBRIDGE('40000000')/...identifyCard(...)"

    // --- 2) Strip off previous operation if present ---
    var sOpSegment = "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyCard(";
    var iOpIndex   = sBasePath.indexOf(sOpSegment);

    if (iOpIndex > -1) {
        // keep only the entity part: "/ZI_WR_R_WEIGHBRIDGE('40000000')"
        sBasePath = sBasePath.slice(0, iOpIndex);
    }

    // --- 3) Now build the correct identifyKorsel path ---
    var sActionPath = sBasePath + "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyKorsel(...)";

    var oAction = oModel.bindContext(sActionPath);
    oAction.setParameter("Korselsnr", sKorselsnr);

    oAction.execute().then(function () {
        var oResultCtx = oAction.getBoundContext();
        if (oResultCtx) {
            oView.setBindingContext(oResultCtx);
        }

        // 🔹🔹 NEW / IMPORTANT PART 🔹🔹
        // Move wizard to the next step after the action finished
        var oWizard = this.oWizard || this.byId("WeighBridgeWizard");
        var oStepK  = this.byId("stepK");

        if (oWizard && oStepK) {
            oWizard.validateStep(oStepK);   // mark step as completed
            // If you still have some "weigh" init logic, call it here:
            // this.onWeighStep3();
            oWizard.nextStep();             // go to next step
        }
        // 🔹🔹 END NEW PART 🔹🔹

    }.bind(this)).catch(function (oError) {
        var sMsg = (oError && oError.message) || "Error calling identifyKorsel";
        MessageToast.show(sMsg);
    }.bind(this));
}
