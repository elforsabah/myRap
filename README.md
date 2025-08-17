_onObjectMatched: function () {
    var oModel = this.getView().getModel();
    var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
    var oNewContext = oListBinding.create({});  // Create new entity context (active, no draft)

    this.getView().setBindingContext(oNewContext);

    // Pre-request the property to initialize it (avoids "must not change before read")
    oNewContext.requestProperty("Vbeln").catch(function () {
        // Ignore if needed
    });
}
