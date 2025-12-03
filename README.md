
var oExtAPI;
var oDialog;

var oActionHandlers = {
    manualattachments: function (oContext, aSelectedContexts) {
        // 'this' is the ExtensionAPI coming from FE
        oExtAPI = this;

        var oTourCtx = aSelectedContexts && aSelectedContexts[0];
        if (!oTourCtx) {
            MessageToast.show("Please select a tour first.");
            return;
        }
        var sTourId = oTourCtx.getProperty("TourId");

        // === REOPEN CASE ===
        if (oDialog) {
            oDialog._currentTourId = sTourId;
            if (oDialog._afterOpenHandler) {
                oDialog.detachAfterOpen(oDialog._afterOpenHandler);
            }
            oDialog._afterOpenHandler = function () {
                applyFiltersAndSearch(sTourId);
            };
            oDialog.attachAfterOpen(oDialog._afterOpenHandler);
            oDialog.open();
            return;
        }

        // === FIRST OPEN ===
        oExtAPI.loadFragment({
            name: "zpdattachment.ext.fragments.GenerateDocDialog",
            controller: oActionHandlers          // for .onDialogChoose / .onDialogCancel
        }).then(function (oLoadedDialog) {
            oDialog = oLoadedDialog;
            oExtAPI.addDependent(oDialog);       // host dialog on FE view
            oDialog._currentTourId = sTourId;

            oDialog._afterOpenHandler = function () {
                applyFiltersAndSearch(sTourId);
            };
            oDialog.attachAfterOpen(oDialog._afterOpenHandler);

            oDialog.open();
        });
    },

    // ... onDialogChoose / onDialogCancel as you already have ...
};
