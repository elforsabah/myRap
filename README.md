// Assuming you already collected the selected items into these arrays:
var aAttachmentItems = aSelectedAttachments;   // your array of attachment objects
var aServiceWRItems  = aSelectedServiceWR;     // your array of service WR objects

// 1) Build JSON strings (as required by action parameters AttachmentItemsjson, ServiceWRItemsjson)
var sAttachmentJson = JSON.stringify(aAttachmentItems);
var sServiceWRJson  = JSON.stringify(aServiceWRItems);

// 2) Get the OData V4 model (Fiori Elements V4: usually the default model on the view)
var oModel = this.getView().getModel();  // sap.ui.model.odata.v4.ODataModel

// 3) Bind the collection-bound action to the Tour entity set
//    Path pattern from metadata: /Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)
var oActionBinding = oModel.bindContext(
    "/Tour/com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)"
);

// 4) Set non-binding parameters exactly as defined in the metadata
oActionBinding.setParameter("AttachmentItemsjson", sAttachmentJson);
oActionBinding.setParameter("ServiceWRItemsjson", sServiceWRJson);

// 5) Execute the action
//    Optional: pass a groupId like "$auto" if you use batching conventions.
oActionBinding.execute("$auto").then(function (oResultContext) {
    // oResultContext is a V4 context pointing to the returned TourType
    // e.g. you can read the returned data if needed:
    var oReturnedTour = oResultContext.getObject();

    sap.m.MessageToast.show("Documents were generated successfully.");

    // Optionally refresh the list / tables if attachments changed
    // This refreshes all bindings of the model (simple & safe):
    oModel.refresh();
}).catch(function (oError) {
    // Handle backend or network errors
    sap.m.MessageBox.error(oError.message || "Error while generating documents.");
});
