onNextFromK: function () {
    var oView       = this.getView();
    var oLocalModel = oView.getModel("local");
    var sKorselsnr  = oLocalModel.getProperty("/korselsnr");

    if (!sKorselsnr) {
        // Simple required check
        MessageToast.show(this.getResourceBundle().getText("stepKLabel")); // or a dedicated i18n text
        return;
    }

    // Pad to 70 chars to match backend internal format
    sKorselsnr = sKorselsnr.padStart(70, "0");
    oLocalModel.setProperty("/korselsnr", sKorselsnr); // keep padded value if you like

    var oModel      = oView.getModel();
    var oSessionCtx = oView.getBindingContext(); // context set by identifyCard in step1

    if (!oSessionCtx) {
        // Safety: should never happen if step1 was successful
        MessageToast.show(this.getResourceBundle().getText("noContext"));
        return;
    }

    // --- Call bound action identifyKorsel on the current session entity ---
    var sActionPath = oSessionCtx.getPath() +
        "/com.sap.gateway.srvd.zsb_wr_weighbrige.v0001.identifyKorsel(...)";

    var oAction = oModel.bindContext(sActionPath);
    oAction.setParameter("Korselsnr", sKorselsnr);

    oAction.execute().then(function () {
        // Use result context from the action
        var oResultCtx = oAction.getBoundContext();
        if (oResultCtx) {
            oView.setBindingContext(oResultCtx);
            oResultCtx.refresh();
        }

        // Optional: handle backend messages (010 = success, 011 = error)
        var aMessages = Messaging.getMessageModel().getData() || [];

        var aSuccess010 = aMessages.filter(function (oMsg) {
            return oMsg.getType() === "Success" &&
                   oMsg.getCode && oMsg.getCode() === "ZWR_WEIGHBRIGE_MESS/010";
        });

        // Example: update transaction state if you want
        if (aSuccess010.length > 0) {
            oLocalModel.setProperty("/transaction_state", "2. Weighing");
        } else {
            oLocalModel.setProperty("/transaction_state", "1. Weighing");
        }

        // Clear any inline error on the field
        this._clearKorselsnrInlineError();

        // Mark step K as valid and move on
        this.oWizard.validateStep(this.byId("stepK"));

        // If you still want to start weighing immediately, keep these two:
        this.onWeighStep3();
        this.oWizard.nextStep();

    }.bind(this)).catch(function (oError) {
        var sErrorMsg = oError && oError.message || "Unknown error";
        this._setKorselsnrInlineError(sErrorMsg);
    }.bind(this));
}

