sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/core/Fragment",
    "sap/m/MessageBox"
], function (MessageToast, Fragment, MessageBox) {
    "use strict";

    var oExtAPI;
    var oDialog;

    // --- wait until MDC FilterBar is initialized ---
    function waitForFilterBarAndSearch(oFilterBar, fnApply) {
        if (!oFilterBar) {
            return;
        }

        // Already initialized → run immediately
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

    function applyFiltersAndSearch(sTourId) {
        // *** IMPORTANT: use oExtAPI.byId, not oDialog.getParent().byId ***
        var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
        var oAttachFB  = oExtAPI.byId("AttachmentFilterBar");

        // Service WR – filter by TourId
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

        // Attachments – just trigger GO
        waitForFilterBarAndSearch(oAttachFB, function () {
            oAttachFB.search();
        });
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI
            var oView = oExtAPI.getView();

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

            if (oDialog) {
                // Re-open: re-wire afterOpen and use new TourId
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

            // FIRST OPEN – use Fragment.load with the VIEW ID as "id"
            Fragment.load({
                id: oView.getId(),                                  // << crucial
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers                          // .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oView.addDependent(oDialog);                        // host dialog on the view
                oDialog._currentTourId = sTourId;

                oDialog._afterOpenHandler = function () {
                    applyFiltersAndSearch(sTourId);
                };
                oDialog.attachAfterOpen(oDialog._afterOpenHandler);

                oDialog.open();
            });
        },

        onDialogChoose: function () {
            // These already worked – unchanged, still use oExtAPI.byId
            var oTopTable    = oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable");

            function getSelectedObjects(oTable) {
                if (!oTable || !oTable.getSelectedContexts) { return []; }
                return (oTable.getSelectedContexts() || []).map(function (oCtx) {
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
