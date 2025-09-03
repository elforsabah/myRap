_onObjectMatched: function () {
  const oModel = this.getView().getModel();
  const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");

  const oCtx = oListBinding.create({});
  this.getView().setBindingContext(oCtx);

  oCtx.created().catch((e) => {
    sap.m.MessageBox.error(e.message || "Failed to create draft");
  });
}
