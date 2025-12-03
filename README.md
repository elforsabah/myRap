sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment"
], function (MessageToast, Fragment) {
    "use strict";

    var oExtAPI;
    var oDialog;

    // --------------------------------------------------------------
    // NEW: Robust function that waits until FilterBars are ready
    // --------------------------------------------------------------
    function waitForFilterBarAndSearch(oFilterBar, fnApply) {
        if (!oFilterBar) return;

        // Case 1: Already initialized → run immediately
        if (oFilterBar.getMetadata().getName() === "sap.ui.mdc.FilterBar" && oFilterBar.isInitialised?.()) {
            fnApply();
            return;
        }

        // Case 2: Not yet → listen to the real initialization event
        var fnHandler = function () {
            oFilterBar.detachEvent("initialized", fnHandler);   // one-time
            oFilterBar.detachEvent("initialise", fnHandler);    // fallback
            fnApply();
        };

        oFilterBar.attachInitialized(fnHandler);
        oFilterBar.attachInitialise(fnHandler);   // older UI5 versions use this
    }

    function applyFiltersAndSearch(sTourId) {
        // Use the dialog's own view to resolve IDs correctly (no prefix issue!)
        var oView = oDialog.getParent(); // oDialog is the Dialog → parent is the Fragment view

        var oServiceFB = oView.byId("ServiceWRFilterBar");
        var oAttachFB   = oView.byId("AttachmentFilterBar");

        // Service WR table – filter by TourId
        waitForFilterBarAndSearch(oServiceFB, function () {
            var mCond = oServiceFB.getFilterConditions() || {};
            mCond.TourId = [{
                operator: "EQ",
                values: [sTourId],
                isEmpty: false
            }];
            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();               // this now really works
        });

        // PrintConfiguration table – just load everything
        waitForFilterBarAndSearch(oAttachFB, function () {
            oAttachFB.search();
        });
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;

            var oTourCtx = aSelectedContexts?.[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

            if (oDialog) {
                // REOPEN case
                oDialog._currentTourId = sTourId;                    // remember for later
                oDialog.detachAfterOpen(oDialog._afterOpenHandler);
                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);
                oDialog.open();
                return;
            }

            // FIRST OPEN
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog"
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                oDialog._currentTourId = sTourId;

                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);

                oDialog.open();
            });
        },

        // Your existing onDialogChoose / onDialogCancel stay exactly the same
        onDialogChoose: function () {
            // ... unchanged code ...
        },

        onDialogCancel: function () {
            if (oDialog) oDialog.close();
        }
    };

    return oActionHandlers;
});
