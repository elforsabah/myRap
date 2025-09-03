_onObjectMatched: async function () {
  try {
    const oModel = this.getView().getModel();
    const oListBinding = oModel.bindList("/ZI_WR_WEIGHBRIDGE");

    // ✅ FE v4: pass all 3 args; give params with creationMode (pick one)
    const oCtx = await this.editFlow.createDocument(
      oListBinding,        // collection binding
      {},                  // optional initial values
      { creationMode: "NewPage" } // or "Inline" / "Dialog" depending on UX
    );

    this.getView().setBindingContext(oCtx);
  } catch (e) {
    sap.m.MessageBox.error(e.message || "Failed to create draft");
  }
}
