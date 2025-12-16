sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (MessageToast, MessageBox) {
    "use strict";

    var oExtAPI;   // ExtensionAPI (ListReport controller extension context)
    var oDialog;   // Dialog instance

    // ============================================================
    // Helpers: find controls inside dialog by local id
    // ============================================================
    function _findControlsInDialogByLocalId(sLocalId) {
        if (!oDialog) {
            return [];
        }

        // Find ANY controls whose id contains the local id
        // (fragment/view prefixes vary, so we match loosely)
        var aFound = oDialog.findAggregatedObjects(true, function (o) {
            try {
                return o && typeof o.getId === "function" && o.getId().indexOf(sLocalId) !== -1;
            } catch (e) {
                return false;
            }
        }) || [];

        return aFound;
    }

    function _preferBestCandidates(aControls, sLocalId) {
        // Put likely candidates first:
        // 1) exact local id endings, 2) macro generated ::Table, 3) anything else
        function score(o) {
            var sId = o.getId();
            var i = 0;

            if (sId.indexOf(sLocalId + "::Table") !== -1) { i += 50; }
            if (sId.endsWith("--" + sLocalId) || sId.endsWith(sLocalId)) { i += 40; }
            if (typeof o.getSelectedContexts === "function") { i += 30; }
            if (o.isA && o.isA("sap.ui.mdc.Table")) { i += 20; }
            if (o.isA && o.isA("sap.ui.table.Table")) { i += 10; }

            return i;
        }

        return aControls.sort(function (a, b) {
            return score(b) - score(a);
        });
    }

    // ============================================================
    // Helpers: selection extraction (works with Macro/MDC/Grid)
    // ============================================================
    function _contextsToObjectsAsync(aCtx) {
        aCtx = aCtx || [];
        return Promise.all(aCtx.map(function (oCtx) {
            if (!oCtx) {
                return Promise.resolve(null);
            }
            // OData V4: requestObject() is the safest
            if (typeof oCtx.requestObject === "function") {
                return oCtx.requestObject();
            }
            // fallback
            if (typeof oCtx.getObject === "function") {
                return Promise.resolve(oCtx.getObject());
            }
            return Promise.resolve(null);
        })).then(function (aObjs) {
            return (aObjs || []).filter(Boolean);
        });
    }

    function _tryGetSelectedContexts(oControl) {
        try {
            if (oControl && typeof oControl.getSelectedContexts === "function") {
                return oControl.getSelectedContexts() || [];
            }
        } catch (e) { /* ignore */ }
        return null;
    }

    function _tryGetSelectedFromGridTable(oControl) {
        // For sap.ui.table.Table style selection
        try {
            if (!oControl) { return null; }
            if (oControl.isA && !oControl.isA("sap.ui.table.Table")) { return null; }

            var aIdx = (typeof oControl.getSelectedIndices === "function") ? (oControl.getSelectedIndices() || []) : [];
            if (!aIdx.length) { return []; }

            var oBinding = (typeof oControl.getBinding === "function") ? oControl.getBinding("rows") : null;
            if (!oBinding || typeof oBinding.getContextByIndex !== "function") { return []; }

            var aCtx = aIdx.map(function (iIndex) {
                return oBinding.getContextByIndex(iIndex);
            }).filter(Boolean);

            return aCtx;
        } catch (e) {
            return null;
        }
    }

    function _getSelectedObjectsFromDialogTableLocalIdAsync(sLocalId) {
        // Find all possible controls that belong to this table
        var aControls = _findControlsInDialogByLocalId(sLocalId);
        aControls = _preferBestCandidates(aControls, sLocalId);

        // Try each candidate until we find one that actually returns selected contexts
        var aSelectedCtx = null;

        for (var i = 0; i < aControls.length; i++) {
            var oCandidate = aControls[i];

            // 1) Macro wrapper or MDC table with getSelectedContexts()
            var aCtx1 = _tryGetSelectedContexts(oCandidate);
            if (aCtx1 && aCtx1.length) {
                aSelectedCtx = aCtx1;
                break;
            }

            // 2) Grid table fallback (selected indices -> binding contexts)
            var aCtx2 = _tryGetSelectedFromGridTable(oCandidate);
            if (aCtx2 && aCtx2.length) {
                aSelectedCtx = aCtx2;
                break;
            }

            // 3) Some macro wrappers expose inner table via getTable()
            try {
                if (typeof oCandidate.getTable === "function") {
                    var oInner = oCandidate.getTable();
                    var aCtx3 = _tryGetSelectedContexts(oInner);
                    if (aCtx3 && aCtx3.length) {
                        aSelectedCtx = aCtx3;
                        break;
                    }
                    var aCtx4 = _tryGetSelectedFromGridTable(oInner);
                    if (aCtx4 && aCtx4.length) {
                        aSelectedCtx = aCtx4;
                        break;
                    }
                }
            } catch (e) { /* ignore */ }
        }

        // If nothing found, return []
        if (!aSelectedCtx || !aSelectedCtx.length) {
            return Promise.resolve([]);
        }

        return _contextsToObjectsAsync(aSelectedCtx);
    }

    // ============================================================
    // Helpers: refresh/rebind macro tables on open
    // ============================================================
    function _rebindDialogTableByLocalId(sLocalId) {
        if (!oDialog) { return; }

        var aControls = _findControlsInDialogByLocalId(sLocalId);
        aControls = _preferBestCandidates(aControls, sLocalId);

        for (var i = 0; i < aControls.length; i++) {
            var o = aControls[i];

            // Macro Table API often has rebindTable()
            if (typeof o.rebindTable === "function") {
                o.rebindTable();
                return;
            }

            // MDC row binding refresh
            try {
                if (typeof o.getRowBinding === "function") {
                    var rb = o.getRowBinding();
                    if (rb && typeof rb.refresh === "function") {
                        rb.refresh();
                        return;
                    }
                }
            } catch (e) { /* ignore */ }

            // If wrapper exposes inner table
            try {
                if (typeof o.getTable === "function") {
                    var inner = o.getTable();
                    if (inner && typeof inner.getRowBinding === "function") {
                        var rb2 = inner.getRowBinding();
                        if (rb2 && typeof rb2.refresh === "function") {
                            rb2.refresh();
                            return;
                        }
                    }
                }
            } catch (e2) { /* ignore */ }
        }
    }

    // ============================================================
    // Controller handlers
    // ============================================================
    var oActionHandlers = {

        // ------------------------------------------------------------
        // Open dialog and bind it to the selected Tour context
        // ------------------------------------------------------------
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a Tour first.");
                return;
            }

            function openDialogForTour() {
                // Bind dialog to selected Tour -> child tables resolve automatically
                oDialog.setModel(oTourCtx.getModel());
                oDialog.setBindingContext(oTourCtx);

                if (!oDialog._afterOpenAttached) {
                    oDialog._afterOpenAttached = true;
                    oDialog.attachAfterOpen(function () {
                        // Rebind both tables when dialog opens
                        _rebindDialogTableByLocalId("AttachmentTable");
                        _rebindDialogTableByLocalId("ServiceWRTable");
                    });
                }

                oDialog.open();
            }

            if (oDialog) {
                openDialogForTour();
                return;
            }

            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                openDialogForTour();
            });
        },

        // ------------------------------------------------------------
        // Choose: read selected rows from both tables and call action
        // ------------------------------------------------------------
        onDialogChoose: function () {
            if (!oExtAPI) {
                MessageBox.error("Extension API not available.");
                return;
            }

            var oModel = oExtAPI.getModel();

            Promise.all([
                _getSelectedObjectsFromDialogTableLocalIdAsync("AttachmentTable"),
                _getSelectedObjectsFromDialogTableLocalIdAsync("ServiceWRTable")
            ]).then(function (aRes) {

                var aAttachmentItems = aRes[0] || [];
                var aServiceWRItems  = aRes[1] || [];

                if (!aAttachmentItems.length && !aServiceWRItems.length) {
                    MessageToast.show("Please select at least one row in one of the tables.");
                    return Promise.resolve(null);
                }

                var oActionBinding = oModel.bindContext(
                    "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
                );

                oActionBinding.setParameter("AttachmentItemsjson", JSON.stringify(aAttachmentItems));
                oActionBinding.setParameter("ServiceWRItemsjson", JSON.stringify(aServiceWRItems));

                return oActionBinding.execute("$auto");

            }).then(function (oResult) {
                if (!oResult) { return; } // user had no selection

                MessageToast.show("Documents were generated successfully.");
                oModel.refresh();

                if (oDialog) {
                    oDialog.close();
                }
            }).catch(function (oError) {
                MessageBox.error(
                    (oError && (oError.message || oError.toString())) || "Error while generating documents."
                );
            });
        },

        onDialogCancel: function () {
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
