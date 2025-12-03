sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (MessageToast, MessageBox) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // single dialog instance

    // Bind the dialog to the selected Tour and trigger searches
    function initDialogForTour(oTourCtx) {
        // Bind dialog to selected Tour (needed for relative _ServiceAssignments)
        oDialog.setModel(oExtAPI.getModel());
        oDialog.setBindingContext(oTourCtx);

        // Trigger searches so data is loaded immediately (no GO press)
        var oServiceFB = oExtAPI.byId("ServiceWRFilterBar");
        if (oServiceFB && oServiceFB.search) {
            oServiceFB.search();
        }

        var oAttachFB = oExtAPI.byId("AttachmentFilterBar");
        if (oAttachFB && oAttachFB.search) {
            oAttachFB.search();
        }
    }

    // Helper: collect selected row objects from a table
    function getSelectedObjectsFromTable(oTable) {
        if (!oTable || !oTable.getSelectedContexts) {
            return [];
        }
        var aContexts = oTable.getSelectedContexts() || [];
        return aContexts.map(function (oCtx) {
            return oCtx.getObject();
        });
    }

    var oActionHandlers = {

        // Called from the “Generate Documents” button on the Tour ListReport
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // in FE V4 this is ExtensionAPI

            // Use the first selected Tour
            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a tour first.");
                return;
            }

            if (oDialog) {
                initDialogForTour(oTourCtx);
                oDialog.open();
                return;
            }

            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers   // .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                initDialogForTour(oTourCtx);
                oDialog.open();
            });
        },

        // User presses "Choose" in the dialog
        onDialogChoose: function () {
            var oTopTable    = oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable");

            var aAttachmentItems = getSelectedObjectsFromTable(oTopTable);
            var aServiceWRItems  = getSelectedObjectsFromTable(oBottomTable);

            if (!aAttachmentItems.length && !aServiceWRItems.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var oModel = oExtAPI.getModel();

            // Bound action on the Tour to which the dialog is bound
            var oTourCtx = oDialog.getBindingContext();
            if (!oTourCtx) {
                MessageBox.error("No tour context available for action execution.");
                return;
            }

            var sActionPath = oTourCtx.getPath() +
                "/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)";

            var oActionBinding = oModel.bindContext(sActionPath);

            oActionBinding.setParameter("AttachmentItemsjson", JSON.stringify(aAttachmentItems));
            oActionBinding.setParameter("ServiceWRItemsjson",  JSON.stringify(aServiceWRItems));

            oActionBinding.execute("$auto").then(function () {
                MessageToast.show("Documents were generated successfully.");
                oModel.refresh(); // refresh ListReport data
            }).catch(function (oError) {
                var sMsg = (oError && oError.message) || "Error while generating documents.";
                MessageBox.error(sMsg);
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
