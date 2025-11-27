this.loadFragment({
    id: "TwoSmartTablesDialog",
    name: "zsb_attachment.ext.fragments.GenerateDocDialog",
    controller: {
        onDialogChoose: function () {
            // get the SmartTables
            var oSmartTop    = oExtAPI.byId("SmartTableTop");
            var oSmartBottom = oExtAPI.byId("SmartTableBottom");

            // underlying inner tables created by SmartTable
            var oTopTable    = oSmartTop.getTable();
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

            var aTopSelected    = getSelectedObjects(oTopTable);
            var aBottomSelected = getSelectedObjects(oBottomTable);

            if (!aTopSelected.length && !aBottomSelected.length) {
                MessageToast.show("Please select at least one row in one of the tables.");
                return;
            }

            // ... your invokeAction logic stays as you have it ...
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
