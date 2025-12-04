onNextFromK: function () {
    var oView       = this.getView();
    var oLocalModel = oView.getModel("local");
    var oModel      = oView.getModel();
    var sKorselsnr  = oLocalModel.getProperty("/korselsnr");

    // ... your checks + padding etc. ...

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

        // your success logic (transaction_state, wizard.nextStep, etc.)

    }.bind(this)).catch(function (oError) {
        // your error handling
        var sMsg = (oError && oError.message) || "Error calling identifyKorsel";
        MessageToast.show(sMsg);
    }.bind(this));
}
