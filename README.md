sap.ui.define([
    "sap/m/MessageToast",
    "sap/m/MessageBox"
], function (MessageToast, MessageBox) {
    "use strict";

    var oExtAPI;    // ExtensionAPI
    var oDialog;    // Dialog instance

    function _refreshMacroTable(sId) {
        if (!oExtAPI || !oExtAPI.byId) { return; }

        var oMacro = oExtAPI.byId(sId) || oExtAPI.byId(sId + "::Table");
        if (!oMacro) { return; }

        // Best case: Macro Table API
        if (typeof oMacro.rebindTable === "function") {
            oMacro.rebindTable();
            return;
        }

        // Fallbacks: inner table / row binding
        var oInner = (typeof oMacro.getTable === "function") ? oMacro.getTable() : oMacro;
        var oRB = oInner && typeof oInner.getRowBinding === "function" ? oInner.getRowBinding() : null;
        if (oRB && typeof oRB.refresh === "function") {
            oRB.refresh();
        }
    }

    function _getSelectedObjects(oTable) {
        if (!oTable || typeof oTable.getSelectedContexts !== "function") {
            return [];
        }
        return (oTable.getSelectedContexts() || []).map(function (oCtx) {
            return oCtx.getObject();
        });
    }

    var oActionHandlers = {

        // --------------------------------------------------------------------
        // Open dialog and bind it to the selected Tour
        // --------------------------------------------------------------------
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this;

            var oTourCtx = aSelectedContexts && aSelectedContexts[0];
            if (!oTourCtx) {
                MessageToast.show("Please select a Tour first.");
                return;
            }

            var openDialogForTour = function () {
                // Bind dialog to selected Tour -> child tables (_Attachments/_ServiceAssignments) auto resolve
                oDialog.setModel(oTourCtx.getModel());
                oDialog.setBindingContext(oTourCtx);

                // Ensure tables refresh each time the dialog opens
                if (!oDialog._afterOpenAttached) {
                    oDialog._afterOpenAttached = true;
                    oDialog.attachAfterOpen(function () {
                        _refreshMacroTable("AttachmentTable");
                        _refreshMacroTable("ServiceWRTable");
                    });
                }

                oDialog.open();
            };

            if (oDialog) {
                openDialogForTour();
                return;
            }

            // First time: load fragment
            oExtAPI.loadFragment({
                name: "zpdattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                openDialogForTour();
            });
        },

        // --------------------------------------------------------------------
        // Choose -> collect selected rows and call RAP action
        // --------------------------------------------------------------------
        onDialogChoose: function () {
            var oTopTable = oExtAPI.byId && oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId && oExtAPI.byId("ServiceWRTable");

            var aAttachmentItems = _getSelectedObjects(oTopTable);
            var aServiceWRItems  = _getSelectedObjects(oBottomTable);

            if (!aAttachmentItems.length && !aServiceWRItems.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            var oModel = oExtAPI.getModel();

            // Keep your existing action call style (your metadata shows it's bound to the Tour collection)
            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            oActionBinding.setParameter("AttachmentItemsjson", JSON.stringify(aAttachmentItems));
            oActionBinding.setParameter("ServiceWRItemsjson", JSON.stringify(aServiceWRItems));

            oActionBinding.execute("$auto").then(function () {
                MessageToast.show("Documents were generated successfully.");
                oModel.refresh();
                if (oDialog) { oDialog.close(); }
            }).catch(function (oError) {
                MessageBox.error(oError && (oError.message || oError.toString()) || "Error while generating documents.");
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
