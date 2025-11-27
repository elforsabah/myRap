sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;   // sap.fe.core.ExtensionAPI
    var oDialog;   // will hold the ONE dialog instance

    var oActionHandlers = {

        /**
         * LR custom action: open (or create) the dialog.
         */
        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // in FE V4 this is ExtensionAPI

            // 1) Dialog already created once -> just reopen
            if (oDialog) {
                oDialog.open();
                return;
            }

            // 2) First time: load fragment once
            oExtAPI.loadFragment({
                name: "zpdattachment.zsbattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers   // .onDialogChoose / .onDialogCancel
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;          // <=== keep reference!
                oExtAPI.addDependent(oDialog);    // clean lifecycle
                oDialog.open();
            });
        },

        /**
         * "Choose" button in the dialog
         */
        onDialogChoose: function () {
            // Tables are building-block tables (mdc.Table) – use ExtensionAPI.byId
            var oTopTable    = oExtAPI.byId("TopTable");
            var oBottomTable = oExtAPI.byId("BottomTable");

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

            // TODO: Call your backend action here with aTopSelected & aBottomSelected

            MessageToast.show(
                "Selected: " + aTopSelected.length + " top, " +
                aBottomSelected.length + " bottom"
            );

            // Close after processing
            if (oDialog) {
                oDialog.close();
            }
        },

        /**
         * "Cancel" button in the dialog
         */
        onDialogCancel: function (oEvent) {
            // Robust: close the parent dialog of the button
            if (oEvent && oEvent.getSource) {
                var oSource = oEvent.getSource();
                var oParent = oSource.getParent();  // for beginButton/endButton this is the Dialog
                if (oParent && oParent.close) {
                    oParent.close();
                    return;
                }
            }

            // fallback – use stored reference
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
