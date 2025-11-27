sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    // will hold the Fiori Elements ExtensionAPI instance
    var oExtAPI;

    // we return this object as the "action handler" + dialog controller
    var oActionHandlers = {

        /**
         * Custom List Report action.
         * Works like your uploadcsv: lazy loads the dialog, then reuses it.
         */
        manualattachments: function (oContext, aSelectedContexts) {
            // In FE V4, "this" is sap.fe.core.ExtensionAPI
            oExtAPI = /** @type sap.fe.core.ExtensionAPI */ this;

            // 1) If dialog already exists, just open it
            var oExistingDialog = oExtAPI.byId("TwoSmartTablesDialog");
            if (oExistingDialog) {
                oExistingDialog.open();
                return;
            }

            // 2) Otherwise lazily load the fragment via ExtensionAPI.loadFragment
            oExtAPI.loadFragment({
                id: "TwoSmartTablesDialog",
                name: "zpdattachment.zsbattachment.ext.fragments.GenerateDocDialog",
                controller: oActionHandlers        // this object handles .onDialogChoose/.onDialogCancel
            }).then(function (oDialog) {

                // Make dialog follow page lifecycle
                oExtAPI.addDependent(oDialog);

                // OPTIONAL: enforce multi-select mode on inner tables
                var fnSetMultiSelect = function (oSmart) {
                    if (!oSmart) { return; }

                    var fn = function () {
                        var oInner = oSmart.getTable();
                        if (oInner && oInner.setMode) {
                            oInner.setMode("MultiSelect");            // sap.m.Table
                        } else if (oInner && oInner.setSelectionMode) {
                            oInner.setSelectionMode("MultiToggle");   // sap.ui.table.Table
                        }
                    };

                    if (oSmart.getTable()) {
                        fn();
                    } else {
                        oSmart.attachInitialise(fn);
                    }
                };

                fnSetMultiSelect(oExtAPI.byId("SmartTableTop"));
                fnSetMultiSelect(oExtAPI.byId("SmartTableBottom"));

                oDialog.open();
            });
        },


        onDialogChoose: function () {
            var oTopTable = oExtAPI.byId("TopTable");
            var oBottomTable = oExtAPI.byId("BottomTable");

            function getSelectedObjects(oInnerTable) {
                if (!oInnerTable) { return []; }
                var aContexts = oInnerTable.getSelectedContexts();
                return aContexts.map(function (oCtx) { return oCtx.getObject(); });
            }

            var aTopSelected = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }


            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            // TODO: call your backend action here with aTopSelected & aBottomSelected
            MessageToast.show("Selected: "
                + aTopSelected.length + " top, "
                + aBottomSelected.length + " bottom");

            // close after processing
            oActionHandlers.onDialogCancel();

        },
        /**
         * Press handler of Cancel button in fragment
         */
        onDialogCancel: function () {
            var oDialog = oExtAPI.byId("TwoSmartTablesDialog");
            if (oDialog) {
                oDialog.close();
            }
        },
    };

    return oActionHandlers;
});
