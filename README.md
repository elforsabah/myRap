sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI; // sap.fe.core.ExtensionAPI

    var oActionHandlers = {

        /**
         * Custom List Report action.
         * Lazy-load dialog, then reuse it.
         */
        manualattachments: function (oContext, aSelectedContexts) {
            // In FE V4, "this" is sap.fe.core.ExtensionAPI
            oExtAPI = this;

            // If the dialog already exists, just open it
            var oExistingDialog = oExtAPI.byId("TwoSmartTablesDialog");
            if (oExistingDialog) {
                oExistingDialog.open();
                return;
            }

            // Otherwise load the fragment once
            oExtAPI.loadFragment({
                name: "zpdattachment.zsbattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers       // onDialogChoose / onDialogCancel live here
            }).then(function (oDialog) {
                // Make dialog follow page lifecycle
                oExtAPI.addDependent(oDialog);
                oDialog.open();
            });
        },

        /**
         * Press handler of "Choose" in the dialog
         */
        onDialogChoose: function () {
            var oTopTable    = oExtAPI.byId("TopTable");
            var oBottomTable = oExtAPI.byId("BottomTable");

            function getSelectedObjects(oTable) {
                if (!oTable) { return []; }
                var aCtx = oTable.getSelectedContexts() || [];
                return aCtx.map(function (oCtx) { return oCtx.getObject(); });
            }

            var aTopSelected    = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            // TODO: call your backend action here with aTopSelected & aBottomSelected

            MessageToast.show(
                "Selected: " + aTopSelected.length + " top, " +
                aBottomSelected.length + " bottom"
            );

            oActionHandlers.onDialogCancel();
        },

        /**
         * Press handler of "Cancel" in the dialog
         */
        onDialogCancel: function () {
            // IDs are prefixed automatically by FE; byId("TwoSmartTablesDialog") still works
            var oDialog = oExtAPI.byId("TwoSmartTablesDialog");
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
