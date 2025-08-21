init: function () {
  sap.ui.core.UIComponent.prototype.init.apply(this, arguments);
  // Hide the FLP shell header when running in Launchpad
  if (sap.ushell && sap.ushell.Container && (sap.ushell.Container as any).getRenderer) {
    try {
      var oRenderer = (sap.ushell.Container as any).getRenderer("fiori2");
      oRenderer.setHeaderVisibility(false, false); // (visible, animate)
    } catch (e) {
      // ignore if not in FLP
    }
  }
}
