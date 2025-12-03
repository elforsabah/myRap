sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment",
    "sap/m/MessageBox"
], function (MessageToast, Fragment, MessageBox) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // Dialog instance

    // ================================================================
    // Helper: resolve a control by its *local* ID
    // (works even with FE/macros prefixes and in fragments)
    // ================================================================
    function resolveControl(sLocalId) {
        var oCtrl = oExtAPI && oExtAPI.byId && oExtAPI.byId(sLocalId);
        if (oCtrl) {
            return oCtrl;
        }

        var mElements = sap.ui.getCore().mElements || {};
        for (var sId in mElements) {
            if (!Object.prototype.hasOwnProperty.call(mElements, sId)) {
                continue;
            }

            // match plain, with "--", and with "::"
            if (
                sId === sLocalId ||
                sId.endsWith("--" + sLocalId) ||
                sId.indexOf(sLocalId) >= 0
            ) {
                return mElements[sId];
            }
        }

        return null;
    }

    // ================================================================
    // Helper: wait until inner sap.ui.mdc.FilterBar is initialized
    // ================================================================
    function waitForFilterBarAndRun(oFilterBar, fnApply) {
        if (!oFilterBar) {
            return;
        }

        var bIsMdc = oFilterBar.isA && oFilterBar.isA("sap.ui.mdc.FilterBar");

        if (bIsMdc && oFilterBar.isInitialised && oFilterBar.isInitialised()) {
            fnApply();
            return;
        }

        var fnHandler = function () {
            if (oFilterBar.detachInitialized) {
                oFilterBar.detachInitialized(fnHandler);
            }
            if (oFilterBar.detachInitialise) {
                oFilterBar.detachInitialise(fnHandler);
            }
            fnApply();
        };

        if (oFilterBar.attachInitialized) {
            oFilterBar.attachInitialized(fnHandler);
        }
        if (oFilterBar.attachInitialise) {
            oFilterBar.attachInitialise(fnHandler);
        }
    }

    // ================================================================
    // Apply filters + trigger GO on both tables
    // ================================================================
    function applyFiltersAndSearch(sTourId) {
        // We want the *inner* MDC FilterBars behind the macros:FilterBar
        // → ID pattern: <macroId>::FilterBar
        var oServiceFB = resolveControl("ServiceWRFilterBar::FilterBar");
        var oAttachFB  = resolveControl("AttachmentFilterBar::FilterBar");

        // --- SERVICE WR: TourId = selected Tour ---
        waitForFilterBarAndRun(oServiceFB, function () {
            var mCond = oServiceFB.getFilterConditions() || {};

            mCond.TourId = [{
                operator : "EQ",
                values   : [sTourId],
                isEmpty  : false
            }];

            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();
        });

        // --- ATTACHMENTS: just trigger GO ---
        waitForFilterBarAndRun(oAttachFB, function () {
            oAttachFB.search();
        });
    }

    // ================================================================
    // Helper: get selected row objects from a macros:Table
    // (the macros table itself already supports getSelectedContexts)
    // ================================================================
    function getSelectedObjectsFromTableId(sTableId) {
        var oTable = oExtAPI && oExtAPI.byId && oExtAPI.byId(sTableId);
        if (!oTable || !oTable.getSelectedContexts) {
            return [];
        }

        var aContexts = oTable.getSelectedContexts() || [];
        return aContexts.map(function (oCtx) {
            return oCtx.getObject();
        });
    }

    // ================================================================
    // Action handlers
    // ================================================================
    var oActionHandlers = {
        // Called from FE action "manualattachments"
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            var sTourId = oTourCtx.getProperty("TourId");

            // --- Dialog already created → just reopen and re-apply filters ---
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

            // --- First time: load fragment, then open ---
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers      // for onDialogChoose / onDialogCancel
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

        // "Choose" button in the dialog
        onDialogChoose: function () {
            var aAttachmentItems = getSelectedObjectsFromTableId("AttachmentTable");
            var aServiceWRItems  = getSelectedObjectsFromTableId("ServiceWRTable");

            if (!aAttachmentItems.length && !aServiceWRItems.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var sAttachmentJson = JSON.stringify(aAttachmentItems);
            var sServiceWRJson  = JSON.stringify(aServiceWRItems);

            var oModel = oExtAPI.getModel();
            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
            oActionBinding.setParameter("ServiceWRItemsjson",  sServiceWRJson);

            oActionBinding.execute("$auto").then(function () {
                MessageToast.show("Documents were generated successfully.");
                oModel.refresh();
            }).catch(function (oError) {
                MessageBox.error(oError.message || "Error while generating documents.");
            });

            if (oDialog) {
                oDialog.close();
            }
        },

        // "Cancel" button
        onDialogCancel: function () {
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
