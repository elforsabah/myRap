var oTopTable    = oDialog && oDialog.byId("AttachmentTable");
var oBottomTable = oDialog && oDialog.byId("ServiceWRTable");



// Assume your custom action is triggered from the Tour ListReport toolbar
// → FE passes ExtensionAPI as `this` so you can get selected Tours:
var aTourContexts = oExtAPI.getSelectedContexts() || [];
if (!aTourContexts.length) {
    MessageToast.show("Please select at least one Tour first.");
    return;
}

// For a collection-bound action, you still need one context to build the path:
var sPath = aTourContexts[0].getPath(); // "/Tour(TourUuid=...)"

// Build action binding
var oActionBinding = oModel.bindContext(
    sPath + "/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
);

// Set parameters
oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
oActionBinding.setParameter("ServiceWRItemsjson",  sServiceWRJson);

// Execute with the tour-context groupId or $auto
oActionBinding.execute("$auto").then(function (oResultContext) {
    MessageToast.show("Documents were generated successfully.");
    oModel.refresh();
}).catch(function (oError) {
    sap.m.MessageBox.error(oError.message || "Error while generating documents.");
});








oExtAPI.loadFragment({
    name: "zpdattachment.ext.fragments.GenerateDocDialog",
    controller: oActionHandlers
});



onDialogChoose: function () {
    // 1) Get the macro tables via the dialog
    var oTopTable    = oDialog && oDialog.byId("AttachmentTable");
    var oBottomTable = oDialog && oDialog.byId("ServiceWRTable");

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

    var sAttachmentJson = JSON.stringify(aTopSelected);
    var sServiceWRJson  = JSON.stringify(aBottomSelected);

    var oModel = oExtAPI.getModel();

    // 2) Get at least one Tour context to bind the bound action
    var aTourContexts = oExtAPI.getSelectedContexts() || [];
    if (!aTourContexts.length) {
        MessageToast.show("Please select at least one Tour first.");
        return;
    }

    var sPath = aTourContexts[0].getPath();

    var oActionBinding = oModel.bindContext(
        sPath + "/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
    );

    oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
    oActionBinding.setParameter("ServiceWRItemsjson",  sServiceWRJson);

    oActionBinding.execute("$auto").then(function (oResultContext) {
        MessageToast.show("Documents were generated successfully.");
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
}



