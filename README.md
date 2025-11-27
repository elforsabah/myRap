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

            // Use ExtensionAPI.loadFragment so FE wires the models correctly
            this.loadFragment({
                id: "TwoSmartTablesDialog",                   // fragment root ID
                name: "project1.ext.fragments.GenerateDocDialog", // <app id>.ext.fragment.<fileName without .xml>
                controller: {
                    // called when user presses Button A
                    onDialogChoose: function () {
                        // Access the inner tables via ExtensionAPI.byId (no view.getId() needed)
                        var oTopTable    = oExtAPI.byId("InnerTopTable");
                        var oBottomTable = oExtAPI.byId("InnerBottomTable");

                        var aTopSelected = oTopTable.getSelectedContexts().map(function (oCtx) {
                            return oCtx.getObject();
                        });
                        var aBottomSelected = oBottomTable.getSelectedContexts().map(function (oCtx) {
                            return oCtx.getObject();
                        });

                        if (!aTopSelected.length && !aBottomSelected.length) {
                            MessageToast.show("Please select at least one row in one of the tables.");
                            return;
                        }

                        // ===== Call backend action with TWO separate inputs =====
                        // Example: RAP unbound action MyService.ProcessTwoSets( TopItems : TopType[], BottomItems : BottomType[] )
                        // Use EditFlow.invokeAction from ExtensionAPI.getEditFlow(). 

                        var oEditFlow = oExtAPI.getEditFlow();
                        var oMainModel = oExtAPI.getModel(); // main OData V4 model

                        oEditFlow.invokeAction("MyService.ProcessTwoSets", {
                            model: oMainModel,               // unbound action -> provide model
                            parameterValues: [
                                { name: "TopItems",    value: aTopSelected },
                                { name: "BottomItems", value: aBottomSelected }
                            ],
                            label: "Process selected items"
                        }).then(function () {
                            MessageToast.show("Processing started.");
                            oDialog.close();
                        }).catch(function (oError) {
                            MessageToast.show(oError.message || "Error while calling backend action.");
                        });
                    },

                    // cancel button
                    onDialogCancel: function () {
                        oDialog.close();
                    }
                }
            }).then(function (oLoadedDialog) {
                oDialog = oLoadedDialog;

                // Register dialog as dependent of the FE page
                oExtAPI.addDependent(oDialog);

                oDialog.open();
            });
        },

        // you can keep your second handler as-is
        manualattachments2: function (oContext, aSelectedContexts) {
            MessageToast.show("Custom handler invoked 2.");
        }
    };
});


LoaderExtensions-dbg.js:331 Uncaught (in promise) Error: resource project1/ext/fragments/GenerateDocDialog.fragment.xml could not be loaded from ../resources/project1/ext/fragments/GenerateDocDialog.fragment.xml. Check for 'file not found' or parse errors. Reason: 
    at LoaderExtensions-dbg.js:331:19
    at SyncPromise-dbg.js:314:14
    at e (SyncPromise-dbg.js:63:4)
    at new r (SyncPromise-dbg.js:230:3)
    at r.then (SyncPromise-dbg.js:313:7)
    at Object.loadResource (LoaderExtensions-dbg.js:323:5)
    at sap.ui.predefine.U.loadTemplate (XMLTemplateProcessor-dbg.js:268:27)
    at Object.Ee [as templateControlFragment] (CommonUtils.ts:1599:25)
    at e.n [as loadFragment] (ExtensionAPI.ts:289:40)
    at e.manualattachments (ListReportExt.js:30:18)





