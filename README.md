sap.ui.define([
    "sap/m/MessageToast"
], function (MessageToast) {
    "use strict";

    // Keep dialog + ExtensionAPI in module scope
    var oDialog;
    var oExtAPI;   // sap.fe.core.ExtensionAPI

    return {
        /**
         * Custom action handler, wired in manifest.json ("press").
         *
         * @param {object} oContext          – FE context (not needed here)
         * @param {sap.ui.model.Context[]} aSelectedContexts – selected rows of the LR table
         */
        manualattachments: function (oContext, aSelectedContexts) {

            // In V4 FE, "this" is the ExtensionAPI, not a controller
            // See sap.fe.core.ExtensionAPI docs (loadFragment, addDependent, getModel, getEditFlow). 
            oExtAPI = this;

            // If dialog already created -> just open
            if (oDialog) {
                oDialog.open();
                return;
            }

            this.loadFragment({
                id: "TwoSmartTablesDialog",
                name: "zsb_attachment.ext.fragments.GenerateDocDialog",
                controller: {
                    onDialogChoose: function () {
                        // get the SmartTables
                        var oSmartTop = oExtAPI.byId("SmartTableTop");
                        var oSmartBottom = oExtAPI.byId("SmartTableBottom");

                        // underlying inner tables created by SmartTable
                        var oTopTable = oSmartTop.getTable();
                        var oBottomTable = oSmartBottom.getTable();

                        function getSelectedObjects(oInnerTable) {
                            if (!oInnerTable) { return []; }

                            var aContexts = [];
                            if (oInnerTable.getSelectedContexts) {
                                // sap.m.Table
                                aContexts = oInnerTable.getSelectedContexts();
                            } else if (oInnerTable.getSelectedIndices) {
                                // sap.ui.table.Table
                                oInnerTable.getSelectedIndices().forEach(function (iIndex) {
                                    var oCtx = oInnerTable.getContextByIndex(iIndex);
                                    if (oCtx) { aContexts.push(oCtx); }
                                });
                            }
                            return aContexts.map(function (oCtx) { return oCtx.getObject(); });
                        }

                        var aTopSelected = getSelectedObjects(oTopTable);
                        var aBottomSelected = getSelectedObjects(oBottomTable);

                        if (!aTopSelected.length && !aBottomSelected.length) {
                            MessageToast.show("Please select at least one row in one of the tables.");
                            return;
                        }

                        
                    },

                    onDialogCancel: function () {
                        oDialog.close();
                    }
                }
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;
                oExtAPI.addDependent(oDialog);

                // OPTIONAL: force multi-select mode on the inner tables
                var fnSetMultiSelect = function (oSmart) {
                    var fn = function () {
                        var oInner = oSmart.getTable();
                        if (oInner && oInner.setMode) {
                            oInner.setMode("MultiSelect");          // sap.m.Table
                        } else if (oInner && oInner.setSelectionMode) {
                            oInner.setSelectionMode("MultiToggle");  // sap.ui.table.Table
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

        // you can keep your second handler as-is
        manualattachments2: function (oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked 2.");
        }
    };
});

