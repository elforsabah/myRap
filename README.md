onInit: function() {
    var oModel = this.getOwnerComponent().getModel();
    var oContext = oModel.createEntry("/ZI_WR_WEIGHINGSESSION");  // Entity set name
    this.getView().setBindingContext(oContext);
},
