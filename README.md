onDialogChoose: function () {
    var oModel = oExtAPI.getModel();

    Promise.all([
        _getSelectedObjectsFromDialogTableLocalIdAsync("AttachmentTable"),
        _getSelectedObjectsFromDialogTableLocalIdAsync("ServiceWRTable")
    ]).then(function (aRes) {

        var aAttachmentItems = aRes[0] || [];
        var aServiceWRItems  = aRes[1] || [];

        if (!aAttachmentItems.length && !aServiceWRItems.length) {
            MessageToast.show("Please select at least one row in one of the tables.");
            return Promise.resolve(null);
        }

        // IMPORTANT: call the bound action *relative to the selected Tour instance*
        var oTourCtx = oDialog && oDialog.getBindingContext();
        if (!oTourCtx) {
            MessageBox.error("No Tour context on dialog. Dialog must be bound to selected Tour before calling the action.");
            return Promise.resolve(null);
        }

        var oActionBinding = oModel.bindContext(
            "com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)",
            oTourCtx
        );

        oActionBinding.setParameter("AttachmentItemsjson", JSON.stringify(aAttachmentItems));
        oActionBinding.setParameter("ServiceWRItemsjson", JSON.stringify(aServiceWRItems));

        // execute() resolves with NO DATA on success in OData V4
        return oActionBinding.execute("$auto");

    }).then(function () {
        // If we get here, execute() succeeded
        MessageToast.show("Documents were generated successfully.");
        oModel.refresh();

        if (oDialog) {
            oDialog.close();
        }
    }).catch(function (oError) {
        MessageBox.error(
            (oError && (oError.message || oError.toString())) || "Error while generating documents."
        );
    });
},
