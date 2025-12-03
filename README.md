sap.ui.define([
    "sap/m/MessageToast",
    "sap/ui/model/Filter",
    "sap/ui/model/FilterOperator"
], function (MessageToast, Filter, FilterOperator) {
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
            oServiceFB.search();  // Triggers immediate load (like pressing GO)
        }

        // --- Attachments: load all (NO TourId filter) ---
        var oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();  // Triggers immediate load (like pressing GO)
        }
    }

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI in FE V4

            // 1) Get selected tour (use first selected row)
            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }
            var sTourId = oTourCtx.getProperty("TourId");

            // 2) Open dialog and pre-filter tables
            if (oDialog) {
                // For reopen: Open first, then apply in afterOpen to ensure controls are ready
                oDialog.attachAfterOpen({ sTourId: sTourId }, function(oEvent) {
                    var sId = oEvent.getParameter("sTourId");
                    applyFiltersAndSearch(sId);
                });
                oDialog.open();
                return;
            }

            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // For initial open: Apply in afterOpen to ensure controls are rendered
                oDialog.attachAfterOpen({ sTourId: sTourId }, function(oEvent) {
                    var sId = oEvent.getParameter("sTourId");
                    applyFiltersAndSearch(sId);
                });

                oDialog.open();
            });
        },

        onDialogChoose: function () {
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

            var aAttachmentItems = aTopSelected;
            var aServiceWRItems  = aBottomSelected;

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
