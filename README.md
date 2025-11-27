sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    var oExtAPI;
    var oActionHandlers = {

        manualattachments: function (oContext, aSelectedContexts) {
            oExtAPI = this; // ExtensionAPI

            // if dialog already exists -> open
            var oExistingDialog = oExtAPI.byId("TwoSmartTablesDialog");
            if (oExistingDialog) {
                oExistingDialog.open();
                return;
            }

            // use VIEW ID as prefix, not "TwoSmartTablesDialog"
            var sViewId = oExtAPI.getView().getId();

            oExtAPI.loadFragment({
                id: sViewId, // <== IMPORTANT
                name: "zpdattachment.zsbattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers
            }).then(function (oDialog) {
                oExtAPI.addDependent(oDialog);
                oDialog.open();
            });
        },

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

            MessageToast.show(
                "Selected: " + aTopSelected.length + " top, " +
                aBottomSelected.length + " bottom"
            );

            oActionHandlers.onDialogCancel();
        },

        onDialogCancel: function () {
            // now this ID resolves to <viewId>--TwoSmartTablesDialog
            var oDialog = oExtAPI.byId("TwoSmartTablesDialog");
            if (oDialog) {
                oDialog.close();
            }
        }
    };

    return oActionHandlers;
});
