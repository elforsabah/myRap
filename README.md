sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox",
    "sap/ui/model/Filter",
    "sap/ui/model/FilterOperator"
], function (MessageToast, MessageBox, Filter, FilterOperator) {
    "use strict";

    var oExtAPI;           // sap.fe.core.ExtensionAPI
    var oDialog;           // ONE dialog instance
    var sSelectedTourId;   // TourId from selected ZC_PDTOUR row

    function applyTourFilters() {
        if (!oExtAPI) {
            return;
        }

        // --- ATTACHMENTS: trigger GO so data is loaded automatically ---
        // For macros:FilterBar, the inner MDC FilterBar has id "<id>::FilterBar"
        var oAttachFB = oExtAPI.byId("AttachmentFilterBar::FilterBar");
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();
        }

        // --- SERVICE WR: filter binding by TourId and trigger read ---
        // For macros:Table, the inner MDC table has id "<id>::Table"
        var oMdcTable = oExtAPI.byId("ServiceWRTable::Table");
        if (oMdcTable) {
            var oBinding = oMdcTable.getRowBinding && oMdcTable.getRowBinding();
            if (oBinding) {
                var aFilters = [
                    new Filter("TourId", FilterOperator.EQ, sSelectedTourId) // <-- adapt property name if needed
                ];

                // Apply application-level filter (AND with user filters)
                oBinding.filter(aFilters, "Application");
            }
        }
    }

    var oActionHandlers = {

        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;   // ExtensionAPI in FE V4

            // Take the first selected tour
            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            var oTourObject = oTourCtx.getObject();
            // >>> Make sure this is the correct field from ZC_PDTOUR <<<
            sSelectedTourId = oTourObject.TourId;

            if (!sSelectedTourId) {
                MessageBox.error("No TourId found on selected tour.");
                return;
            }

            // Dialog already created → just open; afterOpen will apply filters
            if (oDialog) {
                oDialog.open();
                return;
            }

            // First time: load fragment and wire up afterOpen
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // Every time the dialog opens, apply the filters
                oDialog.attachAfterOpen(function () {
                    applyTourFilters();
                });

                oDialog.open();
            });
        },

        onDialogChoose: function () {
            var oTopTable    = oExtAPI.byId("AttachmentTable::Table") || oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable::Table")  || oExtAPI.byId("ServiceWRTable");

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
