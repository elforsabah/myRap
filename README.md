onNextStep: function () {
  const oCtx = this.getView().getBindingContext();  // default V4 model context
  const sVbeln = oCtx && oCtx.getProperty("Vbeln"); // case-sensitive!

  if (!sVbeln) {
    sap.m.MessageToast.show("Please enter Contract ID.");
    return;
  }

  this.byId("wiz").nextStep();
}
