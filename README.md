// FIX: Bind the action relative to the current Tour Context (oTourCtx)
// Note: Remove the leading "/Tour/" and the full namespace if possible, 
// or keep the full namespace but ensure it is relative (no leading slash).
// Usually, for V4, the simple action name works if bound to context.

var oTourCtx = oDialog.getBindingContext(); // Retrieve the context we set earlier

var oActionBinding = oModel.bindContext(
    "com.sap.gateway.srvd.zsd_pdattacments.v0001.generatedocuments(...)", 
    oTourCtx 
);

oActionBinding.setParameter("AttachmentItemsjson", JSON.stringify(aAttachmentItems));
oActionBinding.setParameter("ServiceWRItemsjson", JSON.stringify(aServiceWRItems));

// ... execute ...
