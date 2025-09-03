_onObjectMatched: function () {
    var oModel = this.getView().getModel();
    var oListBinding = oModel.bindList("/ZI_WR_WEIGHINGSESSION", undefined, undefined, undefined, { $$updateGroupId: "weighingGroup" });
    var oNewContext = oListBinding.create({}); // No initial data; let backend manage Sessionid (UUID)
    this.getView().setBindingContext(oNewContext);
    oNewContext.requestProperty(["Vbeln", "Grossweight", "Grossweightunit", "Sessionid"]).catch(function () { });
    this.getView().getModel("local").setProperty("/contractId", "");  // Reset local value
},
