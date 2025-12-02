sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // ONE dialog instance

    var oActionHandlers = {
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // in FE V4 this is ExtensionAPI

            if (oDialog) {
                oDialog.open();
                return;
            }

            oExtAPI.loadFragment({
                name: "zsbattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers   // .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);
                oDialog.open();
            });
        },

        onDialogChoose: function () {
            // Use the IDs from the fragment
            var oTopTable    = oExtAPI.byId("AttachmentTable");
            var oBottomTable = oExtAPI.byId("ServiceWRTable");

            function getSelectedObjects(oTable) {
                if (!oTable || !oTable.getSelectedContexts) { return []; }
                var aCtx = oTable.getSelectedContexts() || [];
                return aCtx.map(function (oCtx) { return oCtx.getObject(); });
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

            // IMPORTANT: use ExtensionAPI, not this.getView()
            var oModel = oExtAPI.getModel();

            var oActionBinding = oModel.bindContext(
                "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
            );

            oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
            oActionBinding.setParameter("ServiceWRItemsjson",  sServiceWRJson);

            oActionBinding.execute("$auto").then(function (oResultContext) {
                var oReturnedTour = oResultContext.getObject();
                sap.m.MessageToast.show("Documents were generated successfully.");
                oModel.refresh();
            }).catch(function (oError) {
                sap.m.MessageBox.error(oError.message || "Error while generating documents.");
            });

            MessageToast.show(
                "Selected: " + aTopSelected.length + " top, " +
                aBottomSelected.length + " bottom"
            );

            if (oDialog) {
                oDialog.close();
            }
        },

        onDialogCancel: function (oEvent) {
            if (oEvent && oEvent.getSource) {
                var oSource = oEvent.getSource();
                var oParent = oSource.getParent();
                if (oParent && oParent.close) {
                    oParent.close();
                    return;
                }
            }
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
