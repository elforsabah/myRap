init: function () {
  sap.ui.core.UIComponent.prototype.init.apply(this, arguments);

  // Hide the FLP shell header when running in Launchpad
  if (sap.ushell && sap.ushell.Container && sap.ushell.Container.getRenderer) {
    try {
      var oRenderer = sap.ushell.Container.getRenderer();
      oRenderer.setHeaderVisibility(false, false); // (visible, animate)
    } catch (e) {
      // ignore if not in FLP
    }
  }
}
