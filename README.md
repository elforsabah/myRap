sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (MessageToast, MessageBox) {
    "use strict";

    var oExtAPI;   // sap.fe.templates.ListReport.ExtensionAPI
    var oDialog;   // Dialog instance

    // ------------------------------------------------------------------------
    // Helper: find the inner sap.ui.mdc.FilterBar by a fragment alias
    // ------------------------------------------------------------------------
    function resolveFilterBar(sAlias) {
        var oFB = null;

        // 1) Try via ExtensionAPI.byId (if available)
        if (oExtAPI && oExtAPI.byId) {
            // sometimes the alias is on the macro wrapper, sometimes on the inner FB
            oFB = oExtAPI.byId(sAlias) || oExtAPI.byId(sAlias + "::FilterBar");
        }

        // If this is the macro wrapper, try to get inner mdc FilterBar
        if (oFB && oFB.getInnerFilterBar && !oFB.isA("sap.ui.mdc.FilterBar")) {
            oFB = oFB.getInnerFilterBar();
        }

        if (oFB && oFB.isA && oFB.isA("sap.ui.mdc.FilterBar")) {
            return oFB;
        }

        // 2) Fallback: scan all elements in Core and look for sap.ui.mdc.FilterBar
        var mElements = sap.ui.getCore().mElements || {};
        Object.keys(mElements).some(function (sId) {
            var oElement = mElements[sId];
            if (
                sId.indexOf(sAlias) !== -1 &&
                oElement &&
                oElement.isA &&
                oElement.isA("sap.ui.mdc.FilterBar")
            ) {
                oFB = oElement;
                return true; // break
            }
            return false;
        });

        return oFB;
    }

    // ------------------------------------------------------------------------
    // Apply filters & trigger search on both tables
    // ------------------------------------------------------------------------
    function applyFiltersAndSearch(sTourId) {
        // Try to resolve the FilterBars *after* they exist in the dialog
        var oServiceFB = resolveFilterBar("ServiceWRFilterBar");
        var oAttachFB  = resolveFilterBar("AttachmentFilterBar");

        // Attachments: just trigger search (no additional filters)
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();
        }

        // Service WR: set TourId = selected Tour and trigger search
        if (oServiceFB &&
            oServiceFB.getFilterConditions &&
            oServiceFB.setFilterConditions &&
            oServiceFB.search) {

            var mCond = oServiceFB.getFilterConditions() || {};
            mCond.TourId = [{
                operator: "EQ",
                values: [sTourId],
                isEmpty: false
            }];

            oServiceFB.setFilterConditions(mCond);
            oServiceFB.search();
        } else {
            // debug helper: you’ll see this if we still don’t find the FilterBar
            jQuery.sap.log.warning(
                "ServiceWR FilterBar not found or no MDC API",
                "",
                "zpdattachment.ext.controller.ListReportExt"
            );
        }
    }

    var oActionHandlers = {
        // --------------------------------------------------------------------
        // Action: open dialog + prefilter tables by selected Tour
        // --------------------------------------------------------------------
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // in FE V4 action handler, `this` is ExtensionAPI

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            var sTourId = oTourCtx.getProperty("TourId");

            // Re-use existing dialog
            if (oDialog) {
                oDialog._currentTourId = sTourId;
                oDialog.open();

                // Run filter logic shortly after open so that inner controls exist
                setTimeout(function () {
                    applyFiltersAndSearch(sTourId);
                }, 0);
                return;
            }

            // First-time load
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers        // for .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                oDialog._currentTourId = sTourId;

                oDialog.attachAfterOpen(function () {
                    // Extra tick to be safe that the macros created all inner controls
                    setTimeout(function () {
                        applyFiltersAndSearch(sTourId);
                    }, 0);
                });

                oDialog.open();
            });
        },

        // --------------------------------------------------------------------
        // Choose button: collect selected rows from both tables & call RAP action
        // --------------------------------------------------------------------
        onDialogChoose: function () {
            var oTopTable    = oExtAPI.byId && oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId && oExtAPI.byId("ServiceWRTable");

            function getSelectedObjects(oTable) {
                if (!oTable || !oTable.getSelectedContexts) {
                    return [];
                }
                var aCtx = oTable.getSelectedContexts() || [];
                return aCtx.map(function (oCtx) {
                    return oCtx.getObject();
                });
            }

            var aAttachmentItems = getSelectedObjects(oTopTable);
            var aServiceWRItems  = getSelectedObjects(oBottomTable);

            if (!aAttachmentItems.length && !aServiceWRItems.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var oModel = oExtAPI.getModel();

            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            oActionBinding.setParameter("AttachmentItemsjson", JSON.stringify(aAttachmentItems));
            oActionBinding.setParameter("ServiceWRItemsjson",  JSON.stringify(aServiceWRItems));

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

        onDialogCancel: function () {
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
