_findInnerMdcTableInDialog: function (sMacroLocalId) {
    if (!oDialog) { return null; }

    var aMdcTables = oDialog.findAggregatedObjects(true, function (o) {
        return o && o.isA && o.isA("sap.ui.mdc.Table");
    }) || [];

    for (var i = 0; i < aMdcTables.length; i++) {
        var sId = aMdcTables[i].getId();
        if (sId && sId.indexOf(sMacroLocalId + "::Table") !== -1) {
            return aMdcTables[i];
        }
    }
    return null;
},

_getSelectedObjectsFromDialogMacroTable: function (sMacroLocalId) {
    var oMdcTable = this._findInnerMdcTableInDialog(sMacroLocalId);
    if (!oMdcTable) { return []; }

    // Preferred (MDC Table)
    if (typeof oMdcTable.getSelectedContexts === "function") {
        return (oMdcTable.getSelectedContexts() || []).map(function (oCtx) {
            return oCtx.getObject();
        });
    }

    // Fallback (if selection API differs)
    if (typeof oMdcTable.getSelectedIndices === "function" && typeof oMdcTable.getRowBinding === "function") {
        var aIdx = oMdcTable.getSelectedIndices() || [];
        var oRB = oMdcTable.getRowBinding();
        if (!oRB) { return []; }

        return aIdx.map(function (iIndex) {
            var oCtx = oRB.getContextByIndex(iIndex);
            return oCtx ? oCtx.getObject() : null;
        }).filter(Boolean);
    }

    return [];
},

onDialogChoose: function () {
    // IMPORTANT: use this.<method>
    var aAttachmentItems = this._getSelectedObjectsFromDialogMacroTable("AttachmentTable");
    var aServiceWRItems  = this._getSelectedObjectsFromDialogMacroTable("ServiceWRTable");

    if (!aAttachmentItems.length && !aServiceWRItems.length) {
        MessageToast.show("Please select at least one row in one of the tables.");
        return;
    }

    var oModel = oExtAPI.getModel();

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
        MessageBox.error(
            (oError && (oError.message || oError.toString())) || "Error while generating documents."
        );
    });
},
