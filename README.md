_applyLtFilter: function () {
  var sVbeln = this.getView().getBindingContext().getProperty("Vbeln");
  var oVBox = this.byId("ltContainer");
  var oBinding = oVBox && oVBox.getBinding("items");
  if (!oBinding) { return; }

  if (sVbeln) {
    var oFilter = new Filter("SalesOrder", FilterOperator.EQ, sVbeln);
    oBinding.filter([oFilter]);          // only items of that contract
  } else {
    oBinding.filter([]);                  // no contract -> clear filter
  }
}
