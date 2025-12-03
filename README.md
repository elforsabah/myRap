sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment"
], function (MessageToast, Fragment) {
    "use strict";

    var oExtAPI;
    var oDialog;

    // --- helper: wait until MDC FilterBar is initialized ---
    function waitForFilterBarAndSearch(oFilterBar, fnApply) {
        if (!oFilterBar) {
            return;
        }

        // Already initialized -> run directly
        if (oFilterBar.isA("sap.ui.mdc.FilterBar") &&
            oFilterBar.isInitialised && oFilterBar.isInitialised()) {
            fnApply();
            return;
        }

        // Otherwise wait for initialization
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
        } else if (oFilterBar.attachInitialise) {
            oFilterBar.attachInitialise(fnHandler);
        }
    }

    // --- NEW: get inner sap.ui.mdc.Table from macros:Table ---
    function getMdcTable(oTableOrMacro) {
        if (!oTableOrMacro) {
            return null;
        }

        // Case 1: already an MDC table
        if (oTableOrMacro.isA && oTableOrMacro.isA("sap.ui.mdc.Table")) {
            return oTableOrMacro;
        }

        // Case 2: macros wrapper exposing getTable()
        if (typeof oTableOrMacro.getTable === "function") {
            var oInner = oTableOrMacro.getTable();
            if (oInner && oInner.isA && oInner.isA("sap.ui.mdc.Table")) {
                return oInner;
            }
        }

        return null;
    }

    // --- NEW: from a macros:Table ID -> MDC FilterBar instance ---
    function getFilterBarForTableId(sTableId) {
        if (!oExtAPI) {
            return null;
        }

        var oMacroTable = oExtAPI.byId(sTableId);
        var oMdcTable   = getMdcTable(oMacroTable);

        if (!oMdcTable || !oMdcTable.getFilter) {
            return null;
        }

        // This is the GLOBAL ID of the FilterBar
        var sFilterBarId = oMdcTable.getFilter();
        if (!sFilterBarId) {
            return null;
        }

        // Resolve the real sap.ui.mdc.FilterBar
        return sap.ui.getCore().byId(sFilterBarId);
    }

    // --- apply filters + trigger GO using the *real* FilterBars ---
    function applyFiltersAndSearch(sTourId) {
        // Resolve via the tables, not by hard-coded FB IDs
        var oServiceFB = getFilterBarForTableId("ServiceWRTable");
        var oAttachFB  = getFilterBarForTableId("AttachmentTable");

        // SERVICE WR – filter by TourId
        waitForFilterBarAndSearch(oServiceFB, function () {
            var mCond = oServiceFB.getFilterConditions() || {};
            mCond.TourId = [{
                operator: "EQ",
                values: [sTourId],
                isEmpty: false
            }];
            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();
        });

        // ATTACHMENTS – just trigger GO once
        waitForFilterBarAndSearch(oAttachFB, function () {
            oAttachFB.search();
        });
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI in FE V4

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

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

            // FIRST OPEN
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
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

        onDialogChoose: function () {
            // You can still fetch tables as before
            var oTopTable    = oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable");

            function getSelectedObjects(oTableLike) {
                var oMdcTable = getMdcTable(oTableLike);
                if (!oMdcTable || !oMdcTable.getSelectedContexts) {
                    return [];
                }
                return (oMdcTable.getSelectedContexts() || []).map(function (oCtx) {
                    return oCtx.getObject();
                });
            }

            var aTopSelected    = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var sAttachmentJson = JSON.stringify(aTopSelected);
            var sServiceWRJson  = JSON.stringify(aBottomSelected);

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
                sap.m.MessageBox.error(oError.message || "Error while generating documents.");
            });

            if (oDialog) {
                oDialog.close();
            }
        },

        onDialogCancel: function () {
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
