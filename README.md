onInit: function () {
var oModel = this.getOwnerComponent().getModel();
var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION");
var oNewContext = oListBinding.create();
this.getView().setBindingContext(oNewContext);
},
