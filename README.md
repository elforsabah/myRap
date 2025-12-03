sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;        // sap.fe.core.ExtensionAPI
    var oDialog;        // ONE dialog instance

    function applyFiltersAndSearch(sTourId) {
        // --- Service WR: Filter by TourId ---
        var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
        if (oServiceFB && oServiceFB.setFilterConditions) {
            var mCond = oServiceFB.getFilterConditions() || {};
            mCond.TourId = [{
                operator: "EQ",
                values: [sTourId],
                isEmpty: false
            }];
            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();  // Triggers immediate load
        }

        // --- Attachments: load all ---
        var oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();  // Triggers immediate load
        }
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

            if (oDialog) {
                // Reopen: Detach old handler, attach new one with current sTourId (closure)
                oDialog.detachAfterOpen(oDialog._afterOpenHandler);
                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);  // Closure captures current sTourId
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);

                oDialog.open();
                return;
            }

            // Initial open
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // Attach handler with closure (captures current sTourId)
                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);  // Always the correct sTourId
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);

                oDialog.open();
            });
        },

        // ... onDialogChoose and onDialogCancel unchanged ...
    };

    return oActionHandlers;
});
